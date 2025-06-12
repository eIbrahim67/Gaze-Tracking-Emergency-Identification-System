# Gaze-Tracking Emergency Identification System

![Gaze-Tracking Emergency Identification System](https://github.com/eIbrahim67/Gaze-Tracking-Emergency-Identification-System/blob/main/gaze_1.png)

## Overview

This project implements a gaze-tracking system integrated with a chatbot to assist users in identifying emergencies through an interactive web interface. The application leverages the WebGazer.js library to track eye movements and determine which quadrant of the screen the user is looking at. Based on prolonged gaze in a quadrant, the system triggers a chatbot to provide relevant information or ask further questions to narrow down the nature of the emergency.

The system divides the screen into four quadrants, each displaying a potential emergency option provided by the chatbot. When a user gazes at a quadrant for a specified duration (e.g., 10 seconds), a notification is displayed, and the chatbot processes the selection to either refine the options or display a final result. The project is designed to be accessible, user-friendly, and responsive, with a focus on real-time interaction.

---

## Features

- **Gaze Tracking**: Utilizes WebGazer.js to track the user's eye movements and determine their focus on specific screen quadrants.
- **Interactive UI**: Displays four quadrants with dynamic content (emergency options) fetched from a chatbot API.
- **Chatbot Integration**: Communicates with a backend API to retrieve and process emergency-related questions and responses.
- **Notifications**: Visual and audio notifications alert the user when a selection is made based on gaze duration.
- **Responsive Design**: Adapts to different screen sizes by dynamically setting the canvas and UI elements to the window's dimensions.
- **Session Persistence**: Saves gaze data across sessions for improved calibration and user experience.
- **Error Handling**: Robust error handling for API calls and response parsing to ensure a smooth user experience.

---

## Prerequisites

To run this project locally, ensure you have the following:

- **Node.js**: Required for running the backend server (if applicable) or for managing dependencies.
- **Web Browser**: A modern browser (e.g., Chrome, Firefox) with webcam support for WebGazer.js.
- **Webcam**: Necessary for eye-tracking functionality.
- **WebGazer.js**: Included via a CDN or local file (see setup instructions).
- **Backend API**: A running instance of the chatbot API (e.g., hosted at `http://localhost:3000/api/v1/prediction/...`).
- **Audio File**: A notification sound file (`notification_sound.mp3`) for audio alerts.

---

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-repo/gaze-emergency-system.git
   cd gaze-emergency-system
   ```

2. **Set Up WebGazer.js**:
   - Include the WebGazer.js library in your project. You can either:
     - Use a CDN by adding the following to your HTML file:
       ```html
       <script src="https://webgazer.cs.brown.edu/webgazer.js"></script>
       ```
     - Or download the library and serve it locally.
   - Ensure the WebGazer.js library is loaded before `main.js`.

3. **Set Up the HTML File**:
   Create an `index.html` file with the following structure:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Gaze-Tracking Emergency System</title>
       <script src="https://webgazer.cs.brown.edu/webgazer.js"></script>
       <script src="main.js"></script>
   </head>
   <body>
       <canvas id="plotting_canvas"></canvas>
       <div id="notification" style="display: none; position: fixed; top: 10px; left: 50%; transform: translateX(-50%); background-color: #333; color: white; padding: 10px; border-radius: 5px;"></div>
       <div id="webgazerNavbar"></div>
   </body>
   </html>
   ```

4. **Add Notification Sound**:
   - Place a `notification_sound.mp3` file in the project directory or update the path in `main.js` to point to a valid audio file.

5. **Set Up the Backend API**:
   - Ensure the chatbot API is running at the specified endpoint (`http://localhost:3000/api/v1/prediction/553eeb0f-0289-496e-9a7d-17072aeb87c3`).
   - Update the endpoint in `main.js` if your API is hosted elsewhere.

6. **Run a Local Server**:
   - Use a simple HTTP server to serve the files (e.g., using Node.js `http-server`):
     ```bash
     npm install -g http-server
     http-server .
     ```
   - Access the application at `http://localhost:8080` (or the port specified by your server).

---

## Usage

1. **Start the Application**:
   - Open the application in a web browser with webcam access enabled.
   - The WebGazer.js library will initialize and prompt for webcam access to begin eye tracking.
   - Follow the on-screen calibration instructions to ensure accurate gaze tracking.

2. **Interact with the System**:
   - The screen is divided into four quadrants, each displaying an emergency option provided by the chatbot.
   - Look at a quadrant for 10 seconds to select it. A notification will appear, and an audio alert will play.
   - The chatbot will process the selection and either:
     - Display new options in the quadrants for further refinement.
     - Show a final result in a centered, rounded gray square.

3. **Restart Calibration**:
   - Click the "Restart" button (if available in the UI) to clear calibration data and restart the process.

4. **End Session**:
   - Closing the browser tab or window will end the WebGazer session and save gaze data (if enabled).

---

## Code Structure

- **main.js**:
  - **Initialization**: Sets up WebGazer.js with ridge regression, a gaze listener, and Kalman filtering.
  - **UI Setup**: Creates a full-screen canvas for calibration and a grid of four squares for displaying options.
  - **Gaze Handling**: Tracks gaze coordinates, determines the quadrant, and triggers notifications after a 10-second gaze threshold.
  - **API Integration**: Sends queries to the chatbot API and processes responses to update the UI.
  - **Notification System**: Displays visual and audio notifications for user selections.
  - **Session Management**: Persists gaze data across sessions and handles session termination.

- **Key Functions**:
  - `window.onload`: Initializes WebGazer and sets up the UI.
  - `handleGaze(data)`: Processes gaze data to determine the quadrant and trigger notifications.
  - `showNotification()`: Displays a notification with the selected option and plays an audio alert.
  - `setupUI()`: Creates the four-quadrant grid for displaying chatbot options.
  - `askQuestion(question)`: Sends a query to the chatbot API and processes the response.
  - `applyTextsToSquares(list)`: Updates the quadrant texts with chatbot-provided options.
  - `updatePageWithList(text)`: Displays the final result in a centered square.
  - `convertToJSON(chatbotResponse)`: Parses the chatbot's response into a structured JSON object.

---

## Configuration

- **Gaze Threshold**: Set in `gazeThreshold` (default: 10,000 ms or 10 seconds). Adjust this value to change how long a user must look at a quadrant to trigger a selection.
- **Away Threshold**: Set in `awayThreshold` (default: 1,000 ms or 1 second). Currently unused but can be implemented to detect when the user looks away.
- **API Endpoint**: Update the URL in the `query` function to match your backend API.
- **Session ID**: The `sessionId` variable ensures consistent chatbot sessions. Generate a new UUID if needed.
- **Notification Sound**: Replace `notification_sound.mp3` with your audio file or update the path in `showNotification`.

---

## Limitations

- **Webcam Dependency**: Requires a webcam and user permission for gaze tracking.
- **Calibration Accuracy**: WebGazer.js accuracy depends on proper calibration and lighting conditions.
- **API Dependency**: The system relies on a running chatbot API. Ensure the endpoint is accessible.
- **Browser Compatibility**: Works best in modern browsers with WebRTC support.
- **Single Selection**: The system currently processes one quadrant selection at a time.

---

## Future Improvements

- **Dynamic Thresholds**: Allow users to adjust gaze and away thresholds via the UI.
- **Enhanced UI**: Add animations or transitions for a smoother user experience.
- **Accessibility Features**: Include keyboard navigation or voice commands for users with limited mobility.
- **Error Recovery**: Implement retry logic for failed API calls.
- **Multi-language Support**: Extend the chatbot to handle multiple languages.
- **Analytics**: Track user interactions and gaze patterns for further analysis.

---

## Troubleshooting

- **WebGazer Not Tracking**: Ensure the webcam is enabled and calibration is completed. Check the console for errors.
- **API Errors**: Verify the API endpoint is correct and the server is running. Check network connectivity.
- **No Notification Sound**: Confirm the `notification_sound.mp3` file exists and is accessible.
- **UI Issues**: Ensure the browser window is not resized during calibration, as this may affect quadrant alignment.

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Make your changes and commit (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request with a detailed description of your changes.

---

## License

```
MIT License © 2025 Ibrahim Mohamed Ibrahim
```

---

## Author & Contact

```
Developed by: Ibrahim Mohamed Ibrahim
GitHub: https://github.com/eIbrahim67
LinkedIn: https://www.linkedin.com/in/eibrahim67
Email: ibrahim.mohamed.ibrahim.t@gmail.com
```

---
