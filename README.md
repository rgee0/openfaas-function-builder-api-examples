# OpenFaaS Pro Function Builder API examples
This repo contains some code examples that show you how to use the [OpenFaaSFunction Builder API](https://docs.openfaas.com/openfaas-pro/builder/) from different languages.

> The Function Builder API provides a simple REST API to create your functions from source code.
> See [Function Builder API docs](https://docs.openfaas.com/openfaas-pro/builder/) for installation instructions

Before attempting to run the examples make sure the pro-builder is port-forwarded to port 8081 on the local host.

```bash
kubectl port-forward \
    -n openfaas \
    svc/pro-builder 8081:8080
```

Save the HMAC signing secret created during the installation to a file `payload.txt` at the root of this repo.
```bash
kubectl get secret \
    -n openfaas payload-secret -o jsonpath='{.data.payload-secret}' \
    | base64 --decode \
    > payload.txt
```

The directory [hello-world](./hello-world/) can be passed as the handler directory to the examples. It contains a javascript handler for a function. The hello-world directory was created by running:
```bash
faas-cli new hello-world --lang node17
```

You can use the `faas-cli` to create any other handler to try these example scripts with.

## Use Python to call the pro-builder
The [python-request](./python-request/) directory has an example on how to invoke the Function Builder API from python. Run the `build.py` script with the required flags to turn a function handler into a container image.

The python script uses the [Requests](https://requests.readthedocs.io/en/latest/) package so you will have to install that to run the example.
```
python -m pip install requests
```

Run the script
```bash
python3 python-request/build.py \
    --image docker.io/functions/hello-world:0.1.0 \
    --handler ./hello-world \
    --lang node17
```

## Use NodeJS to call the pro-builder
The [nodejs-request](./nodejs-request/) directory has an example on how to invoke the Function Builder API from NodeJS. Run the `index.js` script with the required arguments to turn a function handler into a container image.

Install the required packages
```bash
cd nodejs-request
npm install
```

The script takes three arguments:
- The docker image name to build
- A directory with the handler for the function
- The language or template to use

```bash
node nodejs-request/index.js \
    'docker.io/functions/hello-world:0.1.0' \
    ./hello-world \
    node17
```