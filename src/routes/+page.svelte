<script lang="ts">
	let files = $state<FileList>();
	let error = $state('');

	$effect(() => {
		if (files && files.length > 0) {
			error = '';
		}
	});

	const downloadCSV = (data: string, filename: string) => {
		const blob = new Blob([data], { type: 'text/csv' });
		const url = window.URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.setAttribute('href', url);
		a.setAttribute('download', `${filename}.csv`);
		a.click();
		a.remove();
	};

	const handleSubmit = (e: Event) => {
		e.preventDefault();

		if (files) {
			// Grab the uploaded file
			const file = files.item(0);

			if (file) {
				file
					// Get the text of the uploaded file as a Promise
					.text()
					// Parse the file contents to get the log data and download as a CSV
					.then((fileContent) => {
						const parser = new DOMParser();
						const document = parser.parseFromString(fileContent, 'application/xml');
						const errorNode = document.querySelector('parsererror');
						if (errorNode) {
							error = 'Unable to parse file. Are you sure this is a valid .xle file?';
						} else {
							error = '';
							const filename = file.name.substring(0, file.name.lastIndexOf('.'));
							const logs = Array.from(document.getElementsByTagName('Log'));
							const fields = Array.from(logs[0].children ?? []).map((child) => child.tagName);
							let csv = fields.join(',');

							logs.forEach((log) => {
								csv += '\r\n';
								csv += Array.from(log.children)
									.map((logField) => logField.textContent)
									.join(',');
							});

							downloadCSV(csv, filename);
						}
					});
			}
		} else {
			error = 'Please select a file to convert';
		}
	};
</script>

<div class="container py-5" style="max-width: 600px;">
	<h1 class="mb-4">Levelogger XLE to CSV Converter</h1>

	<form onsubmit={handleSubmit}>
		<div class="mb-3">
			<label for="xle-file" class="form-label">
				Upload a .xle file of your Levelogger data to convert to a .csv of your log data:
			</label>
			<input
				bind:files
				id="xle-file"
				name="xle"
				type="file"
				class="form-control {error ? 'is-invalid' : ''}"
			/>
			{#if error}
				<div class="invalid-feedback">{error}</div>
			{/if}
		</div>
		<div class="mb-3">
			<input type="submit" value="Convert to CSV" class="btn btn-primary" />
		</div>
	</form>
</div>
