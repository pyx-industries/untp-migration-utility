# pyx-untp-migration

# 📘 Pyx UNTP Migration Tool

This project is designed to programatically read and convert Verifiable Credentials (JSON Bodies) from one version to another allowing for automated migration and when upgrading to the latest UNTP Version.

The Code requests a mapping file with a set of instructions on the transformations that need to be made to the JSON. e.g.
```json
{
    "rules": [
        {
            "action": "replace",
            "path": "@context.type[]",
            "from": "https://test.uncefact.org/vocabulary/untp/dte/0.5.0/",
            "to": "https://test.uncefact.org/vocabulary/untp/dte/0.6.0/"
        },
        {
            "action": "move",
            "from": "credentialSubject.type",
            "to": "credentialSubject.facility.type"
        }, {
            "action": "move",
            "from": "credentialSubject.id",
            "to": "credentialSubject.facility.id"
        },
        {
            "action": "move",
            "from": "credentialSubject.name",
            "to": "credentialSubject.facility.name"
        },
        {
            "action": "change",
            "path": "credentialSubject.type[1]",
            "value": "FacilityRecord"
        }
    ]
}
```

## 📂 Folder Structure
```
untp-migration-utility/
├── untp_migrator.py                        # main python script for comand line integration
├── app.py                                  # front end interface for migration tool.
├── utilities/json_migration_utility.py     # core module the provides tranformation logic.
├── maps/                                   # json instruction sets for converting credentials from version to version
└── app-data/                               # dependencies from the front end application
```

# 🚀 Setup & Usage

1. Clone the repository /pyx-apps/ in Ubuntu 24.04
2. Install Python Extensions:
    - Python Debugger
    - Python
    - Pylance
    - Python Environment venv
    - pip install flask in venve

## CLI usage: untp_migrator.py

Run migrations from the terminal:
```
python3 untp_migrator.py \
  -m mapping_file_path/mapping.json \
  -i input_file_path/input.json \
  -o output_file_path/out.json
```

Options

- `-m`, `--mapping`: Path to the mapping rules JSON file.
- `-i`, `--input`: Path to the input JSON file to be transformed.
- `-o`, `--output`: Path where the transformed output JSON will be written.
- `--strict` (optional, if enabled in your wrapper): Fail if a move source path is missing (instead of skipping).

Example:

```
python3 untp_migrator.py -m examples/mapping.json -i examples/input.json -o out.json
```

## Web UI usage: app.py

Launch the local website:
```
python3 app.py
```

What happens:

1. A local server starts on http://127.0.0.1:<port>/
1. Your default browser opens automatically
1. Upload mapping.json and input.json, click Transform
1. The transformed JSON appears in the output area
1. Click Download to save the output
1. Stop the server with: `Ctrl + C`


## 🚀 Testing

To test the credentials and check UNTP Compliance:
1. Upload output credentials to [UNTP Playground](https://test.uncefact.org/untp-playground)

## 📂 Input & Output Examples

- **Single credential transformation**  
  ```
  untp-migration-utility/test/input/in1-0-5-0.json
  → untp-migration-utility/test/output/out1-0-6-0.json
  ```
  Note: using map: `untp-migration-utility/maps/dfr_0_5_0_to_0_6_0.json`

- **BULK credential transformation**  
  ```
  TBD
  ```
---

## ✅ Notes
- This code is primarily designed as a proof of concept to explore how UNTP Migration can happen quickly and seamlessly in the future across environments.
