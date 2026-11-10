# Fprime Python Reference: Implementing Component in Python

`fprime-python` provides an autocoder for automatically binding F Prime C++ code to Python. It is designed to work within the Python interpreter allowing access to standard F Prime components and topologies.

`fprime-python-reference` (this repository) provides a reference project that builds on top of `fprime-python`.

## Example Components

The `fprime-python-reference` repository includes example components that demonstrate how to implement F Prime components in Python. These components are located in the `FprimePythonReference/Components` directory and include:

1. ActiveImager: An `active` component that uses Python and OpenCV to capture/downlink camera images
2. PythonGc: A `queued` component that demonstrates how to telemeter Garbage Collection information
3. PythonTcpCom: A `passive` component that implements a communication interface using TCP sockets

> [!CAUTION]
> This reference will use your system's camera.

> [!NOTE]
> The `fprime-python` rate group driver is also used in this reference.

## Building

First, clone the project using `fprime-bootstrap`, which will ensure that `requirements.txt` is properly installed:

```bash
pip install -U fprime-bootstrap
fprime-bootstrap clone https://github.com/fprime-community/fprime-python-reference.git
```

Next, source the environment created by `fprime-bootstrap` and build/run as usual:

```bash
. fprime-venv/bin/activate
fprime-util generate
fprime-util build
```

> [!NOTE]
> The build for `fprime-python` projects is slower than a C++ build as it also autocodes and compiles the Python bindings.

The build installs the `fprime_py` extension module and every Python file of the deployment to `build-artifacts/python`. Python files are reinstalled by every `fprime-util build`.

## Running

The flight software runs inside a Python interpreter, so it is started separately from the GDS. The reference's communication component, `PythonTcpCom`, listens for a TCP connection; the GDS must therefore connect as a client.

First, run the GDS in the root of the project:

```bash
. fprime-venv/bin/activate
fprime-gds --ip-client
```

> [!NOTE]
> `fprime-gds.yml` sets `--no-app`, which starts the GDS without running an executable.

Next, in a second terminal, run the flight software with the same Python that built it:

```bash
. fprime-venv/bin/activate
python3 build-artifacts/python/fsw_main.py
```

`fprime-python-runner` does the same from any directory of the project. `--hostname` and `--port` set the address `PythonTcpCom` listens on (default `127.0.0.1:50000`); give the GDS the matching `--ip-address` and `--ip-port`.

## Testing

With the GDS and the flight software running, run the integration tests from the root of the project:

```bash
pytest test/int --dictionary build-artifacts/*/FprimePythonReference_ReferenceDeployment/dict/ReferenceDeploymentTopologyDictionary.json
```
