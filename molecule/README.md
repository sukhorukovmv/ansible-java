# molecule test

This README file demonstrate molecule work.

## Getting Started

### Requirements
The main requirements are documented in `requirements.txt` but this demo assumes you have a few other things installed including
- `docker`
- `python`

Practice safe python. Create a `virtualenv`.
```
virtualenv venv --python=python3.8
```

Activate virtual env.
```
source ./venv/bin/activate
```

Install the python requirements
```
pip install -r requirements.txt
```

Make sure Molecule is installed 
```
molecule --help
```

Run all tests
```
molecule test
```

Run scenario
```
molecule test -s scenario_name
```
