````markdown
# Setup Guide

> **This file is read by the automated evaluation pipeline. Be precise and complete.**

## Prerequisites

Before you begin, ensure you have the following installed:

- [ ] Node.js 18+
- [ ] Bun 1.0+
- [ ] IBM Bob (local or remote)
- [ ] Git
- [ ] A modern web browser
- [ ] Tesseract OCR (optional, required for OCR-based processing of scanned documents)

### Node.js

Install Node.js 18 or later.

Verify the installation:

```bash
node --version
```
````

### Bun

Install Bun 1.0 or later.

Verify the installation:

```bash
bun --version
```

### IBM Bob

IBM Bob must be available either locally or through a remote endpoint configured using the Bob environment variables.

The following environment variables are used to configure the Bob integration:

- `BOB_API_BASE_URL`
- `BOB_API_KEY`
- `BOB_MODEL`

### Tesseract OCR

Tesseract OCR is optional. It is used when OCR processing is required for scanned medical documents or images.

#### Windows

Install Tesseract OCR and make sure the Tesseract executable is available to the application.

Verify the installation:

```bash
tesseract --version
```

#### Ubuntu / Debian

```bash
sudo apt update
sudo apt install tesseract-ocr
```

Verify:

```bash
tesseract --version
```

#### macOS

Using Homebrew:

```bash
brew install tesseract
```

Verify:

```bash
tesseract --version
```

### Database

No PostgreSQL installation is required for the hackathon application.

The project uses a JSON file store for application data.

## Environment Variables

Copy `.env.example` to `.env` and fill in the required values:

```bash
cp .env.example .env
```

The application uses the following environment variables:

| Variable             | Description                                                         | Required                     |
| -------------------- | ------------------------------------------------------------------- | ---------------------------- |
| `PORT`               | Port on which the application server runs                           | Yes                          |
| `BOB_API_BASE_URL`   | Base URL of the IBM Bob API/service                                 | Yes                          |
| `BOB_API_KEY`        | API key used to authenticate with IBM Bob                           | Yes                          |
| `BOB_MODEL`          | IBM Bob model used by the application                               | Yes                          |
| `GEMINI_API_KEY`     | API key used for Google Gemini AI functionality                     | Yes                          |
| `DATABASE_URL`       | Database/storage configuration used by the application              | Yes                          |
| `OTP_DEV_MODE`       | Enables development OTP behaviour for local testing                 | Yes                          |
| `OTP_EXPIRY_MINUTES` | Number of minutes before an OTP expires                             | Yes                          |
| `SUPABASE_URL`       | Supabase project URL used by the application                        | Yes                          |
| `SUPABASE_KEY`       | Supabase access key used by the application                         | Yes                          |
| `UPLOAD_FOLDER`      | Directory used for uploaded medical documents                       | Yes                          |
| `[PROJECT VARIABLE]` | Additional project-specific configuration defined in `.env.example` | As defined in `.env.example` |
| `[PROJECT VARIABLE]` | Additional project-specific configuration defined in `.env.example` | As defined in `.env.example` |

> **Important:** Use the exact variable names and values expected by the project's `.env.example`. Never commit the real `.env` file, API keys, or other secrets to Git.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/Sodha-Manoharsinh/bob-ai-hackathon-MedCoders
```

### 2. Enter the project directory

```bash
cd bob-ai-hackathon-MedCoders
```

### 3. Install dependencies

Using Bun:

```bash
bun install
```

The project does not require a separate frontend installation because the application serves the frontend/static files directly from the application server.

### 4. Configure environment variables

Create the local environment file:

```bash
cp .env.example .env
```

Open `.env` and configure the required values:

```text
PORT=3000
UPLOAD_FOLDER=uploads
GEMINI_API_KEY=AIzaSyB-0bigh7NRSzgr-GNOWrMLvFd5U1Fw4Ms

```

Do not commit `.env` to the repository.

## Running the Application

Start the application in development mode:

```bash
bun run dev
```

The application will be available at:

```text
http://localhost:3000
```

The project uses a single application server, so a separate frontend development server is not required.

## Running Tests

Run the project's lint/type-check command:

```bash
bun run lint
```

The lint command performs the TypeScript type-check using:

```bash
tsc --noEmit
```

A successful command indicates that the TypeScript source passes the project's configured type checking.

## Quick Demo

The quickest way to demonstrate Medical Report AI is:

### 1. Start the application

```bash
bun run dev
```

### 2. Open the application

Open:

```text
http://localhost:3000
```

### 3. Log in

Use the application's OTP-based authentication flow.

When development OTP mode is enabled, the OTP is printed to the application/server console for local testing.

### 4. Upload a medical report

Upload a supported medical document such as a medical PDF or image.

The application processes the document and extracts relevant medical information.

### 5. Explore the results

Review the extracted medical information and generated insights.

The application supports the core Medical Report AI workflow, including:

- Medical document upload
- PDF/image processing
- OCR support where required
- Clinical information extraction
- AI-generated patient summaries
- Clinical analysis
- Document-grounded question answering
- Clinical report generation

### 6. Generate the clinical report

Use the report-generation functionality to create a downloadable PDF report from the processed medical information.

## Troubleshooting

| Issue                            | Solution                                                                                                                                                             |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Module not found                 | Run `bun install` again from the project root and make sure all dependencies are installed correctly.                                                                |
| IBM Bob API connection refused   | Check `BOB_API_BASE_URL`, confirm that the IBM Bob service is running or reachable, and verify the configured endpoint.                                              |
| IBM Bob authentication/API error | Check `BOB_API_KEY` and `BOB_MODEL` in `.env` and confirm that they contain valid values.                                                                            |
| OTP is not received              | When `OTP_DEV_MODE` is enabled, check the application/server console for the development OTP. Also verify `OTP_EXPIRY_MINUTES` and the authentication configuration. |
| `413` upload error               | Check the uploaded file size and the application's configured upload/file-size limits. Try a smaller supported document.                                             |
| Tesseract is missing             | Install Tesseract OCR for your operating system and verify it with `tesseract --version`. OCR is optional when the uploaded document does not require OCR.           |
| Port conflict                    | If port `3000` is already in use, stop the process using it or configure the application to use another available port through `PORT`.                               |
| `data/db.json` permission error  | Make sure the application has read/write permission for the `data` directory and the `data/db.json` file.                                                            |
| Gemini API error                 | Verify `GEMINI_API_KEY` in `.env` and confirm that the configured API key is valid.                                                                                  |
| Supabase connection error        | Verify `SUPABASE_URL` and `SUPABASE_KEY` and make sure the configured Supabase project is accessible.                                                                |
| Uploaded files cannot be saved   | Check that the directory specified by `UPLOAD_FOLDER` exists and that the application has permission to write to it.                                                 |

## Important Security Notes

- Never commit `.env` to Git.
- Never expose API keys in frontend source code.
- Never include real patient medical records in the public repository.
- Use synthetic or appropriately anonymized documents for demonstrations.
- The application is a hackathon prototype and should not be treated as a production clinical decision-making system without additional security, validation, privacy, and regulatory controls.

```

```
