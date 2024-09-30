# Permafrost Report Downloader

A Bash script to download an Archivematica normalization report in CSV format.

## Purpose

If you normalize your files for preservation and/or access, it is good practice to click on the report icon next to the `Approve Normalization` job to review the normalization report. This report opens in a new tab and presents 10 entries at a time, summarizing the normalization attempts and outcomes, among other format-based information. If you are processing large transfers, it may be challenging to review files 10 at a time.

## Requirements

There are only two commands that need to be installed:

- `curl`
- `xmllint`

## Usage

```bash
./report_downloader ${REPORT_URL} ${NUMBER_PAGES} ${CSRF_TOKEN} ${SESSION_ID} > report.csv
```

- `REPORT_URL`: the URL for the first page of the normalization report.

- `NUMBER_PAGES`: the total number of pages in the report.

- `CSRF_TOKEN`: a token stored in your browser's cookies that prevents unauthorized commands from being sent by 
                trusted users. For security, treat this cookie like a password: it should not be shared.

- `SESSION_ID`: a unique identifier given to indicate a user's web session. For security, treat this cookie like a password: it should not be shared.

**Note:** If you log out or are logged out of Archivematica, you will need to retrieve a new CSRF token and session ID.

After the script is done, you will find that a file named `report.csv` was created in the directory where you ran the
script. You can change the name of the file by editing the name at the end of the command.

If running the command returns a "Permission denied" error, you may need to first run `chmod +x report_downloader.sh` 
to grant permission to execute the script.

## Getting the variables

### Report URL

The URL must end in `\` and would be structured as: `https://your-Archivematica-instance.scholarsportal.info/ingest/normalization-report/uuid-of-the-package-in-the-Ingest-tab/`.

### Number of pages

To calculate, divide the total number of items in the report by `10`. If there is a remainder, plus 1 
(e.g., `1143/10 = 114.3`, then total pages = `115`).

**Note:** (1) there is currently an upper limit of `10,000,000` for the page count, (2) if no page count is given, the 
script will only return the last page of results.

### CSRF token and Session ID

1. Log into Archivematica
2. In that tab, open the Developer Console for your browser and navigate to the Storage tab (example, _below_)

![Fetching token](.img/img1.png)

3. Select the Archivematica entry under `Cookies`
4. Copy the values for `csrftoken` and `sessionid`

## Bringing it all together

1. Download the script and note the path to the folder where the script is stored.
2. Gather your variable information.
3. Open your preferred CLI tool.
4. Navigate to the folder where the script is stored: `cd /path/to/folder`
5. Call the script using your gathered variables: `./report_downloader.sh ${REPORT_URL} ${NUMBER_OF_PAGES} csrftoken=${CSRF_TOKEN} sessionid=${SESSION_ID} > report.csv`
6. Locate `report.csv` in the folder where the script is stored and review! 
