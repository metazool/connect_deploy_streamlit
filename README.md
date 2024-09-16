# connect_deploy_streamlit

Minimal, "hello world" test of git-based continuous deployment of Streamlit to Posit Connect.

You can use the [rsconnect](https://docs.posit.co/rsconnect-python/) python client to deploy, but the server side claims you can do a continuous pull from a repository, and it would be nice to set that up and stop thinking about it.

Question here is about how we store credentials for data storage?

* [Posit Connect - streamlit docs](https://docs.posit.co/connect/user/streamlit/)
* [rsconnect-python CLI](https://docs.posit.co/rsconnect-python/)
* ["Git-backed content" for Connect](https://docs.posit.co/connect/user/git-backed/)
* [Streamlit app starter kit](https://github.com/streamlit/app-starter-kit)

## Steps

Use `rsconnect` to create a manifest, commit it back to the repo. This adds everything, so we could happily point the manifest at a subdirectory rather than creating a UI project

```
python -m venv ~/streamlit_env 
. ~/streamlit_env/bin/activate
pip install -r requirements.txt
pip install rsconnect
rsconnect write-manifest streamlit ./
git add manifest.json
```
