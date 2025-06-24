Pesapal Mobile Money Payment System
A full-stack web application demonstrating a mobile money payment integration using the Pesapal API. This system allows users to initiate payments via major Tanzanian mobile networks (M-Pesa, Tigo Pesa, Airtel Money, HaloPesa) directly from a web interface, triggering a USSD/STK push for PIN confirmation on their mobile device.

Features
Modern, responsive frontend built with HTML and Tailwind CSS.

Interactive network selection with visual feedback.

Real-time polling for transaction status updates.

Python (Flask) backend to securely handle API requests.

Direct integration with the Pesapal v3 API for initiating real transactions.

Clear separation of frontend and backend concerns.

Tech Stack
Frontend: HTML, JavaScript, Tailwind CSS
--   Backend: Python 3, Flask, Requests

Payment Gateway: Pesapal API v3

How It Works: The Payment Flow
Initiation: The user enters their phone number and amount on the frontend and clicks "Pay".

Backend Request: The frontend sends the payment details to the Python/Flask backend.

Pesapal Authentication: The backend authenticates with Pesapal using your consumer key and secret to get a temporary bearer token.

IPN Registration: The backend registers a notification URL (IPN Listener) with Pesapal. This is the endpoint Pesapal will call to send status updates.

Order Submission: The backend submits the payment order to Pesapal, including the user's phone number and amount.

STK Push: Pesapal communicates with the respective Mobile Network Operator (e.g., Vodacom) to push a USSD prompt to the user's phone.

User Confirmation: The user enters their mobile money PIN on their phone to authorize the transaction.

IPN Callback: Once the transaction is complete (or fails), Pesapal sends a notification to the registered IPN URL on your backend, updating the payment status.

Frontend Polling: The frontend periodically polls the backend to get the latest transaction status and updates the UI accordingly (e.g., showing "Payment Successful").

Prerequisites
Python 3.6+

pip for installing Python packages

A Pesapal Merchant Account (Sandbox or Live) to get your credentials.

A publicly accessible server (e.g., Heroku, AWS, DigitalOcean) for live deployment, as Pesapal requires a public URL for callbacks.

Setup and Installation
Clone the repository:

git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name

Set up the Backend:

Navigate to the backend directory.

It's recommended to create and activate a virtual environment:

python3 -m venv venv
source venv/bin/activate 
# On Windows, use: venv\Scripts\activate

Install the required Python packages:

pip install Flask Flask-Cors requests

Configuration
The backend requires your Pesapal credentials and public domain to function.

Open backend.py and update the following configuration variables:

# Replace with your actual credentials from the Pesapal portal
PESAPAL_CONSUMER_KEY = "YOUR_PESAPAL_CONSUMER_KEY"
PESAPAL_CONSUMER_SECRET = "YOUR_PESAPAL_CONSUMER_SECRET"

# Replace this with your publicly accessible backend domain (MUST be HTTPS for live)
YOUR_PUBLIC_DOMAIN = "https://your-live-app.com"

# Set the appropriate API domain
PESAPAL_API_DOMAIN = "https://cybqa.pesapal.com" # Sandbox URL
# PESAPAL_API_DOMAIN = "https://pay.pesapal.com" # Live URL

For better security, it is highly recommended to set these as environment variables rather than hardcoding them in the file.

(For live deployment) Update the Frontend API URL:

Open index.html.

Find the fetch call and change the URL from http://127.0.0.1:5000 to your public backend URL.

// Change this line in index.html
const response = await fetch('https://your-live-app.com/api/make-payment', {
    // ...
});

Running the Application
Start the Backend Server:

python backend.py

The server will start on http://127.0.0.1:5000.

Launch the Frontend:

Open the index.html file in your web browser.

You can now test the payment flow.

Deployment Notes
For production, use a WSGI server like Gunicorn or uWSGI instead of Flask's built-in development server.

Your backend must be deployed to a server with a public HTTPS URL for Pesapal's IPN callbacks to work.

Ensure you configure CORS correctly on your backend to only allow requests from your frontend's domain.
