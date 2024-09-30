# Permafrost Report Downloader

A Bash script to download an Archivematica ingestion report in CSV format.

## Requirements

There are only two commands that need to be installed:

- `curl`
- `xmllint`

## Usage

```bash
./report_downloader ${REPORT_URL} ${NUMBER_PAGES} ${CSRF_TOKEN} ${SESSION_ID} > report.csv
```

- `REPORT_URL`: the URL for the first page of the normalization report. Note that this URL must end in `/`

- `NUMBER_PAGES`: the total number of pages in the report.

- `CSRF_TOKEN`: a token stored in your browser's cookies that prevents unauthorized commands from being sent by 
                trusted users.

- `SESSION_ID`: a unique identifier given to indicate a user's web session

**Note:** If you log out or are logged out of Archivematica, you will need to retrieve a new CSRF token and session ID.

After the script is done, you will find that a file named `report.csv` was created in the directory where you ran the
script. You can change the name of the file by editing the name at the end of the command.

If running the command returns a "Permission denied" error, you may need to first run `chmod +x report_downloader.sh` 
to grant permission to execute the script.

## Getting the variables

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
