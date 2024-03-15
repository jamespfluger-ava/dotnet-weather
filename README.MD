## What is this?

This is an absolute minimal ASP.NET application that responds to GET requests by using the command `curl http://localhost:5000/weatherforecast` when the application is running locally.

### How to build with melange and apko

#### 1. Generate a key to sign the package with
```
docker run --rm -v "${PWD}":/work --entrypoint=melange --workdir=/work ghcr.io/wolfi-dev/sdk keygen
```

#### 2. Create an APK for the weather API
```
docker run --rm --privileged -v "${PWD}":/work cgr.dev/chainguard/melange build melange.yaml --arch x86_64 --signing-key melange.rsa
```

#### 3. Bundle that APK with Wolfi into a file
```
docker run -v "$PWD":/work cgr.dev/chainguard/apko build sample-apko.yaml weather-api-tag weather-api.tar
```

