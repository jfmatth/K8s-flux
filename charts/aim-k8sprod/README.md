# aim-k8sprod

This is the HELM chart to deploy the original version of AIM (v1.x) to a K8's cluster.

There have minimal changes to the code and files to allow it to run

## Static Files
Whitenoise - The use of whitenoise to serve static files directly from django instead of the nginx_config.ini file

## WSGI server
waitress-serve - Instead of uwsgi as a WSGI server, use the pure python, easier for containers version of waitress-serve, from the Pylons project

## Loadprices script
Instead of the script doing any thinking, use a K8s cronjob object to schedule the run.  The python code still does some work.

