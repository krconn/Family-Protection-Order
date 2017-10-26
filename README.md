# Family Protection Order - Prototype

This is a [Django](http://www.djangoproject.com) project imported from the [bcgov/eDivorce](https://github.com/bcgov/eDivorce) repository as a starting point.

ToDo: Update documentation as the product evolves.

_Following is the original project documentation._

---
# eDivorce

This is a [Django](http://www.djangoproject.com) project forked from the [openshift/django-ex](https://github.com/openshift/django-ex) repository.

Family/Protection Order project was developed by the British Columbia Ministry of Justice to help self represented litigants fill out the paperwork for their Provincial Family forms filings.  It replaces existing fillable PDF forms with a friendly web interface. Fiscal 17/18 will focus on Protection Orders and 18/19 will merge other filings (e.g. Custody, maintainence and access).

The steps in this document assume that you have access to an OpenShift deployment that you can deploy applications on.

## Local development

Prerequesites:
* Docker
* Python 3.5

To run this project in your development machine, follow these steps:

1. (optional) Create and activate a [virtualenv](https://virtualenv.pypa.io/) (you may want to use [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/)).

2. Clone this repo:

    `git clone https://github.com/bcgov/eDivorce.git`

3. Install dependencies:

    `pip3.5 install -r requirements.txt`

4. Create an environment settings file by copying `.env.example` to `.env` (`.env` will be ignored by Git)

5. Create a development database:

    `python3.5 ./manage.py migrate`

6. Load questions from fixtures:
  
    `python3.5 ./manage.py loaddata edivorce/fixtures/Question.json`

7. If everything is alright, you should be able to start the Django development server:

    `python3.5 ./manage.py runserver 0.0.0.0:8000`

8. Start the [Weasyprint server](https://hub.docker.com/r/aquavitae/weasyprint/) server on port 5005

    1. Bind the IP address 10.200.10.1 to the lo0 interface on your Mac computer.  Weasyprint has been configured to use this IP address to request CSS files from Django *(You should only have to do this once)*.
        ```
        sudo ifconfig lo0 alias 10.200.10.1/24
        ```

    1. Start docker
        ```
        docker run -d -p 5005:5001 aquavitae/weasyprint
        ```

9. Open your browser and go to http://127.0.0.1:8000, you will be greeted with the eDivorce homepage.  In dev mode, you can log in with any username and the password 'divorce'.


## OpenShift deployment

See: `openshift/README.md`

## Data persistence

For local development a SQLite database will be used.  For OpenShift deployments data will be stored in a PostgreSQL database, with data files residing on a persistent volume.

## License

    Copyright 2017 Province of British Columbia

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
