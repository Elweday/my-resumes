# LaTeX Resume Automation

This repository automates the process of rendering a LaTeX resume and uploading it to Google Drive using GitHub Actions.

## Getting Started

Follow these steps to set up and run this project:

### 1. Existing or Creating New GCP Project

1. **Create a New GCP Project:**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/).
   - Click on the project dropdown in the top bar and select "New Project".
   - Fill in the project name and other required details, then click "Create".

2. **Use an Existing GCP Project:**
   - Select your existing project from the project dropdown in the Google Cloud Console.

### 2. Create a New Service Account

1. **Create Service Account:**
   - In the Google Cloud Console, navigate to **IAM & Admin** > **Service Accounts**.
   - Click **Create Service Account**.
   - Fill in the details (Service account name, ID, etc.) and click **Create**.
   - Keep the email of the service account you'll need it.

2. **Grant Permissions:**
   - In the "Grant this service account access to the project" section, add the role `Editor`.

3. **Create and Download Key:**
   - After creating the service account, go to the **Keys** section.
   - Click **Add Key** > **Create New Key**.
   - Select JSON format and download the key file.

### 3. Make a New Google Drive Folder and Give the Service Account Access

1. **Create Folder:**
   - Go to [Google Drive](https://drive.google.com/) and create a new folder.
   - Get the folder id , you'll need it later `https://drive.google.com/drive/folders/<FOLDER_ID>`


2. **Share Folder:**
   - Right-click on the new folder and select **Share**.
   - Add the service account email and give it **Editor** permissions.
   - Click **Send** to save the changes.

### 4. Get Token and Encode it in Base64

1. **Encode Credentials:**
   - Open the downloaded JSON key file and copy its contents.
   - Use a base64 encoding tool (e.g., `base64` command in Linux, online Base64 encoders) to encode the JSON content.
   - Example command for Linux/Mac:
    ```sh
    cat path/to/your/service-account-file.json | base64
    ```
> If you're on windows you can do it online but it may jeoprdize your token, DO IT ON YOUR OWN RISK 
   
5. Change Repository Variables (FOLDER_ID, CREDS)
- Go to your GitHub repository.
- Navigate to Settings > Secrets and variables > Actions.
- Click New repository secret.
- Add `FOLDER_ID` (Get it from the drive folder URL)
- Click New repository secret.
- ADD `CREDS` paste the `base64` encoded JSON key content 

## Workflow
The GitHub Actions workflow (.github/workflows/main.yml) performs the following steps:

### Build Job:
- Checks out the code.
- Installs necessary LaTeX packages.
- Renders the resume.tex file to resume.pdf.
- Uploads the resume.pdf as an artifact.

### Upload Job:
- Downloads the resume.pdf artifact from the build job.
- Verifies the presence of the resume.pdf file.
- Uploads the resume.pdf file to the specified Google Drive folder.

## Conclusion
With these steps, you can automate the process of generating and uploading a LaTeX resume to Google Drive using GitHub Actions. Make sure to verify that the service account has appropriate permissions and that the secrets are correctly set in your GitHub repository.
