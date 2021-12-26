# Mitsuba Conda Dockers

**Dockerfiles** to build [Mitsuba](https://github.com/mitsuba-renderer/mitsuba) 0.6.0 with bindings to a python version installed with **Conda**. These dockers build Mitsuba from a [fork](https://github.com/Quartenia/mitsuba) I made to build on `python3 scons` and with all the files preconfigured to build the python bindings with Conda.

Inspired by [mitsuba-docker](https://github.com/benjamin-heasly/mitsuba-docker).

The dockers are organized in the folder `/dockerfiles`.

Currently only the docker for ubuntu18.04 is available but more versions will be added soon.

> ❗Currently supports running mitsuba through the command-line interface, but enabling mitsuba GUI should be possible following [mitsuba-docker](https://github.com/benjamin-heasly/mitsuba-docker)

## Build and run the dockers

to build a docker:

```
docker build -t your-name/mitsuba-conda ./dockerfiles/ubuntu18.04
```

to run

```
docker run -it your-name/mitsuba-conda
```

## Render a scene

You could run the docker mounting the folder `your-folder` containing `your-scene.xml` to render that scene.

`your-folder` will be mounted in `/app`, and we can render the scene with the command `mitsuba /app/your-scene.xml`.

Two ways of doing this:

1. either run the container in the terminal
   ```
   docker run --mount type=bind,source="$(pwd)"/your-folder,target=/app -it your-name/mitsuba-conda
   ```
   and then run this mitsuba command
   ```
   mitsuba /app/your-scene.xml
   ```
2. or run the container without a terminal but provide the mitsuba command already

   ```
   docker run --mount type=bind,source="$(pwd)"/your-folder,target=/app your-name/mitsuba-conda /app/your-scene.xml
   ```

> 📸 Both ways will output `your-scene.exr` in the folder containing `your-scene.xml`

### Test

To test rendering a scene.

1.  download the Cornell Box scene `cbox.zip` from http://mitsuba-renderer.org/download.html.

2.  Unzip `cbox.zip`, then in `cbox.xml` change the second line `<scene version="0.4.0">` to `<scene version="0.6.0">`.

3.  Finally run:

    ```
    docker run --mount type=bind,source="$(pwd)"/cbox,target=/app your-name/mitsuba-conda mitsuba /app/cbox.xml
    ```

## Mitsuba-python bindings

The dockers are already set up so that conda installs a suitable version of boost-python required by Mitsuba.
To test the mitsuba python binding run:

```
docker run your-name/mitsuba-conda python3.7 -c "import mitsuba;from mitsuba.core import Vector;print(Vector(1.0, 2.0, 3.0))"

```

> Make sure to specify the correct version of python installed with Conda. By default, the command python will invoke python2.7 interpreter.
