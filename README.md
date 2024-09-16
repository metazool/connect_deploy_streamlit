# connect_deploy_streamlit

Minimal, "hello world" test of git-based continuous deployment of Streamlit to Posit Connect.

You can use the [rsconnect](https://docs.posit.co/rsconnect-python/) python client to deploy, but the server side claims you can do a continuous pull from a repository, and it would be nice to set that up and stop thinking about it.

Question here is about how we store credentials for data storage?

* [Posit Connect - streamlit docs](https://docs.posit.co/connect/user/streamlit/)
* [rsconnect-python CLI](https://docs.posit.co/rsconnect-python/)
* ["Git-backed content" for Connect](https://docs.posit.co/connect/user/git-backed/)
* [Streamlit app starter kit](https://github.com/streamlit/app-starter-kit)

## Steps

Use `rsconnect` to create a manifest, commit it back to the repo. This adds everything, so we could happily point the manifest at a subdirectory rather than creating a UI project: "first make the directory your working directory. Then call writeManifest."

```
python -m venv ~/streamlit_env 
. ~/streamlit_env/bin/activate
pip install -r requirements.txt
pip install rsconnect
rsconnect write-manifest streamlit ./
git add manifest.json
```

## Glitches 

First attempt yields us `Cannot find compatible environment: no compatible Local environment with Python version 3.12.2`.

There's an API endpoint which should tell us which pythons are available, it needs an API key which takes a few clicks in the Connect UI plus a capture solve.

```
API_KEY="your api key"

curl --silent --show-error -L --max-redirs 0 --fail \
    -X GET \
    -H "Authorization: Key ${API_KEY}" \
    "https://connect.example.com/__api__/v1/server_settings/python"
```

This shows we've only got 3.9.6 available, and becomes a future support request; for now let's downgrade.
Option to tweak the version in `manifest.json`, better to create a matching python environment locally (our default system python on these locked down VMs is 3.9.18...)

`rsconnect write-manifest streamlit ./ --overwrite`

It's happy with the micro version difference ("republish" by scrolling down the `Info` tab and "Update now").#

Switch "Sharing" to "all users - login required" on the `Access` tab - this is as open as we can go

There's a `Vars` tab associated with the deployment, this looks like our best bet for keeping object store API keys in.

Deployment _appears_ successful but all we see at the endpoint is a pulsing loading page, and it looks like reading the logs requires a level of access that we don't yet have



