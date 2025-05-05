	// Attach event listeners after the card content loads
	function setupAudioSpans() {
		// Select all spans with the class "word-audio"
		const audioSpans = document.querySelectorAll(".word-audio");

		// Track the currently playing audio element
		let currentlyPlayingAudio = null;

		// Attach click event listeners to each span
		audioSpans.forEach(span => {
			span.addEventListener("click", function () {
				// Get the audio ID from the data-audio-id attribute
				const audioId = span.getAttribute("data-audio-id");

				if (audioId) {
					// Prefix the ID with "--" to match the CSS variable naming convention
					const cssVariableName = `--${audioId}`;

					// Retrieve the audio URL from the CSS variable
					let audioUrl = getComputedStyle(document.documentElement)
						.getPropertyValue(cssVariableName)
						.trim() // Remove any whitespace
						.replace(/^url\(["']?/, "") // Remove "url(" wrapper
						.replace(/["']?\)$/, ""); // Remove ")" wrapper

					console.log("Retrieved Audio URL:", audioUrl);

					// Check if the URL is valid
					if (audioUrl) {
						try {
							// Create a new Audio object using the URL
							const audioElement = new Audio(audioUrl);

							// Stop and reset any currently playing audio
							if (currentlyPlayingAudio && currentlyPlayingAudio !== audioElement) {
								currentlyPlayingAudio.pause();
								currentlyPlayingAudio.currentTime = 0; // Reset playback position
							}

							// Play the clicked audio
							audioElement.play().catch(error => {
								console.error("Audio playback failed:", error);
							});

							// Track the currently playing audio
							currentlyPlayingAudio = audioElement;
						} catch (error) {
							console.error("Error creating or playing audio:", error);
						}
					} else {
						console.error(`Audio URL not found for CSS variable: ${cssVariableName}`);
					}
				} else {
					console.error("CSS variable name is missing for this span.");
				}
			});
		});
	}

	// Ensure the script runs after the DOM is ready
	if (document.readyState === "loading") {
		document.addEventListener("DOMContentLoaded", setupAudioSpans);
	} else {
		setupAudioSpans();
	}

	// Function to toggle the image class between 'image_cropped' and 'image_expanded'
	function toggleImageClass() {
		var image = document.getElementById('id_image');
		if (image) {
			image.classList.toggle('image_expanded');
		}
	}

	// Function to set up the front card event listener
	function setupFrontCard() {
		var image = document.getElementById('id_image');
		if (image) {
			image.addEventListener('click', toggleImageClass);
			console.log('Click event listener added to front image.'); // For debugging
		} else {
			console.log('Front image not found!'); // For debugging
		}
	}

    // Ensure this code runs after the front card is rendered
    setupFrontCard();
