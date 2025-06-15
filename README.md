Real-Time Data Ingestion Pipeline (Google Colab)

colab link: https://colab.research.google.com/drive/1N7cmAf6IvdlfVaau3jGkGoEkuYPM8JXF?usp=sharing

This repository hosts a Google Colab notebook, Real_Time_Data_ingestion.ipynb, which implements a real-time data ingestion pipeline using Apache Spark and Delta Lake. The pipeline generates synthetic customer data, stores it in a Delta table, and sends HTML email notifications with execution summaries, all within the Google Colab environment.
Table of Contents

Overview
Features
Prerequisites
Setup Instructions
Usage
File Structure
Email Notifications
Notes
Troubleshooting
Contributing
License

Overview
The Real_Time_Data_ingestion.ipynb notebook is designed to run in Google Colab and simulates a real-time data ingestion pipeline. It:

Generates synthetic customer data (Name, Address, Email) using the Faker library.
Stores data in a Delta Lake table for reliable, versioned storage.
Runs on a configurable schedule (default: every 2 minutes).
Sends HTML email notifications with execution details and data previews.
Displays interactive HTML tables in Colab for monitoring.
Provides version tracking and basic data analysis.

This project is ideal for demonstrating real-time data processing in a cloud-based, notebook environment.
Features

Synthetic Data Generation: Creates customer records with timestamps.
Delta Lake Storage: Uses Delta Lake for ACID-compliant, versioned data storage.
Scheduled Execution: Runs periodically (default: every 2 minutes) for a set duration (default: 10 minutes).
Email Notifications: Sends formatted HTML emails with pipeline summaries and data previews.
Interactive Visualizations: Renders data and pipeline status in HTML tables within Colab.
Error Handling: Includes robust error handling and logging.
Data Analysis: Provides basic statistics (e.g., total records, unique names/emails).

Prerequisites
To run the notebook in Google Colab, you need:

A Google account to access Colab.
An internet connection for installing dependencies and sending emails.
Python libraries (installed via the notebook):
pyspark==3.4.0
delta-spark==2.4.0
faker==19.6.2
schedule==1.2.0
pandas


Java 8 (installed automatically by the notebook).
A Gmail account with an App Password for email notifications (optional).

Setup Instructions

Clone or Download the Repository:
git clone https://github.com/<your-username>/<your-repo-name>.git

Or download Real_Time_Data_ingestion.ipynb from this repository.

Open in Google Colab:

Upload the notebook to Google Colab: Open in Colab.
Save a copy to your Google Drive: File > Save a copy in Drive.


Install Dependencies:

Run the first cell to install pyspark, delta-spark, faker, schedule, and Java 8.
The notebook automatically handles dependency installation.


Configure the Pipeline:

Modify the config dictionary in the third cell:config = {
    'delta_table_path': '/content/delta-table/customer_data',  # Colab storage path
    'timezone': 'Asia/Kolkata',  # Your timezone
    'records_per_iteration': 5,  # Records per iteration
    'schedule_interval_minutes': 2,  # Interval between runs
    'email': {
        'enabled': True,  # Enable email notifications
        'smtp_server': 'smtp.gmail.com',
        'smtp_port': 587,
        'sender_email': 'your-email@gmail.com',  # Your Gmail address
        'sender_password': 'your-app-password',  # Gmail App Password
        'recipient_emails': ['recipient@example.com']  # Recipient email(s)
    }
}


Gmail App Password: Generate an App Password in your Google Account settings (Security > 2-Step Verification > App Passwords) for sender_password.


Run the Notebook:

Execute cells sequentially to set up and run the pipeline.



Usage

Initialize the Pipeline:

Run the cell with:pipeline = ColabDeltaDataPipeline(config)




Test a Single Iteration:

Run:pipeline.run_pipeline_iteration()


This generates fake data, appends it to the Delta table, displays results in HTML, and sends an email (if enabled).


Run Scheduled Pipeline:

Start the scheduled pipeline:pipeline.start_scheduled_pipeline(duration_minutes=10)


Runs every 2 minutes (configurable) for 10 minutes (adjustable).
Stop early with:pipeline.stop_pipeline()




View Data and Analysis:

View the latest Delta table contents:pipeline.get_latest_table_contents()


The final cell provides data analysis (total records, date range, unique names/emails).


Cleanup:

Stop the Spark session:pipeline.cleanup()


Run the cleanup function:cleanup_pipeline()





File Structure
├── Real_Time_Data_ingestion.ipynb  # Google Colab notebook with the pipeline
├── LICENSE                        # MIT License file (recommended)
└── README.md                      # This file

Note: The Delta table is stored in /content/delta-table/customer_data/ within Colab’s temporary storage during execution.
Email Notifications

If email.enabled = True, the pipeline sends HTML emails after each iteration.
Emails include:
Execution summary (records appended, table version, timestamp).
Newly added data (up to 5 records).
Latest table contents (top 10 records).
Version information.


View a sample email here.

Notes

Colab Storage: Data in /content/delta-table/ is temporary and persists only during the Colab session. To persist data, modify delta_table_path to use Google Drive (e.g., /content/drive/MyDrive/delta-table/).
Email Setup: Use a Gmail App Password for sender_password to avoid authentication issues.
Customization: Extend the pipeline by adding data fields in generate_fake_data or modifying the schedule in config.
Performance: Optimized for small-scale ingestion. For larger datasets, adjust records_per_iteration or Spark settings (e.g., spark.driver.memory).

Troubleshooting

Dependency Errors:
Ensure the first cell installs all libraries.
Restart the Colab runtime (Runtime > Restart runtime) if issues occur.


Email Issues:
Verify SMTP settings and App Password.
Disable notifications (email.enabled = False) if not needed.


Delta Table Errors:
Ensure delta_table_path is accessible.
Delete the Delta table directory and restart if corrupted.


Pipeline Stops:
Colab may disconnect after inactivity. Use shorter duration_minutes or keep the session active.



Contributing
Contributions are welcome! To contribute:

Fork the repository.
Create a new branch:git checkout -b feature/your-feature


Modify the notebook or add features.
Commit changes:git commit -m "Add your feature"


Push to the branch:git push origin feature/your-feature


Open a pull request.

Please test changes in Colab and document them clearly.
License
This project is licensed under the MIT License. See the LICENSE file for details.
