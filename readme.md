# Team Career Camp Placement Cell Web Application

This is a web application for Team Career Camp's Placement Cell. The application allows employees of the company to manage student data, interviews, and results. It also provides a feature to fetch real available jobs in India for React/Node.js and allows users to download all the data in CSV format.

## Features

- Sign Up and Sign In: Employees can sign up and sign in to access the application.
- Student Management: Add, edit, and view student details, including batch, college, status, and course scores.
- Interview Management: Schedule and manage interviews with company details and dates.
- Result Management: Allocate students to interviews and mark their results.
- External Jobs List: Fetch real available jobs in India for React/Node.js using open APIs.
- Download CSV: Download a complete CSV file containing all student interview data with relevant details.
- Professional Design: The application has a professional and clean user interface.

## Tech Stack

The application is built using the following technologies:

- Flask: A Python web framework for building the backend server.
- HTML/CSS: For creating the frontend user interface and styling.
- Bootstrap: For additional styling and responsive design.

## Setup

1. Clone the repository to your local machine.
2. Create a virtual environment (optional but recommended).
3. Install the required dependencies using the following command:
   ```
   pip install -r requirements.txt
   ```
4. Run the application with the following command:
   ```
   python app.py
   ```
5. Access the application in your web browser by visiting `http://localhost:5000`.

## Usage

1. Sign up or sign in to access the application.
2. Use the navigation bar to manage students, interviews, and results.
3. View and manage student details, interviews, and results on their respective pages.
4. Use the "External Jobs List" page to view available jobs in India for React/Node.js.
5. Click on the "Download CSV" button on the student list page to download all student interview data in CSV format.

## Folder Structure

The folder structure of the application is organized as follows:

- `static`: Contains static files such as CSS stylesheets and JavaScript files.
- `templates`: Contains HTML templates for rendering the pages.
- `app.py`: The main Flask application file.
- `students.csv`: A CSV file to store student data (dummy data for demonstration purposes).

## Notes

- The application uses dummy data for students, interviews, and results for demonstration purposes. Replace the dummy data with your own database logic (e.g., SQLite, JSON, etc.) in the `app.py` file.

## Credits

- The application uses Bootstrap for styling.
