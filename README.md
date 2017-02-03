# How to: Flask and Gunicorn (Python 3.5.3) in OpenShift

A step by step guide. [Check the commit history of the `step-by-step` branch!](https://github.com/aaossa/openshift-diy-python-how-to/commits/step-by-step). In each step, the bold part will take you to the commit in the `step-by-step` branch (read the comments!). The 'Go to `x`' will show you the source of `x`.

1. [**Create the `pre_build` script**](https://github.com/aaossa/openshift-diy-python-how-to/commit/07f18beff83429c318dd2dc4efcae0dd4c2a352b): This script will be executed before the build step. Time to install Python 3.5.3 (if it's not there already), create your virtualenv fodler (same) and clean up your temporary directory. [Go to `pre_build`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/.openshift/action_hooks/pre_build)
2. [**Create the `build` script**](https://github.com/aaossa/openshift-diy-python-how-to/commit/576cbc11b75daceb9043767e9ce35390e930b9ec): In this step, our requirements are installed in our virtual environment. We'll use Flask and Gunicorn (more on this later). [Go to `build`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/.openshift/action_hooks/build)
3. [**Create a "Hello world!" Flask app**](https://github.com/aaossa/openshift-diy-python-how-to/commit/8039816e83986c19612159b699e10bec86ac0b7b): This Flask application will be served by Gunicorn later. It's simple... it's just a "Hello world!"... nothing too interesting. [Go to `demo.py`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/demo.py)
4. [**Create `requirements.txt`**](https://github.com/aaossa/openshift-diy-python-how-to/commit/e25bc4b94db31dc975626e903c4b1c2ea8a58f0e): This requirements will be installed in the build step. I put here everything that my app needs to run in Python. So I'll put here "Flask". [Go to `requirements.txt`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/requirements.txt)
5. [**Create `dev_requirements.txt**](https://github.com/aaossa/openshift-diy-python-how-to/commit/188438799034e2ea758f99b8cf9364b39a4e6ebe): In this file I put everything that my app needs to be executed from bash scripts (like Gunicorn and Virtualenv) and for testing (like Pytest). In this case I put "Gunicorn". [Go to `dev_requirements.txt`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/dev_requirements.txt)
6. [**Create the `start` script**](https://github.com/aaossa/openshift-diy-python-how-to/commit/3a570367916c91d63ab5820c2920d801435e47f2): The start script launches the application. Here I activate the virtual environment and use Gunicorn to serve in the background. Read the script, I liked it. [Go to `start`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/.openshift/action_hooks/start)
7. [**Create the `stop` script**](https://github.com/aaossa/openshift-diy-python-how-to/commit/d2fa97cefe239f839c97df27b2614aaeca7efcfe): This script uses the pid file created by Gunicorn in the start script to kill the process properly. [Go to `stop`](https://github.com/aaossa/openshift-diy-python-how-to/blob/step-by-step/.openshift/action_hooks/stop)

This template provides Python 3.5.3, a "Hello world!" Flask app and a Gunicorn configuration that serves the Flask app. Now that everything that needs to be said has already been said... Take this code and use it with just one command

```
rhc app-create <app_name> diy-0.1 --from-code https://github.com/aaossa/openshift-diy-python-how-to.git
```

**NOTE:** To install Python, activate the environment, etc., **the `pre_build` script must be triggered**. How to do that? Just **push** a commit. Install Python from source takes **a lot of time (~10+ minutes)**. To set up Travis CI to deploy automatically to OpenShift read the wiki!

###### Why I did this? This steps can be applied to any language, server configuration, etc. in OpenShift. Just use the `action_hooks` scripts to set up everything. I liked it