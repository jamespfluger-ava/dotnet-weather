## What is this?

This is an absolute minimal ASP.NET application that responds to GET requests by using the command `curl http://localhost:5000/weatherforecast` when the application is running locally.

### How to build with melange and apko

**This assumes you perform the commands within the `/src` directory**

#### 1. Generate a key to sign the package with
```
docker run --rm --privileged --volume "${PWD}":/work --workdir=/work cgr.dev/chainguard/melange keygen weather-api.rsa
```

#### 2. Create an APK for the weather API
```
docker run --rm --privileged --volume "${PWD}":/work --workdir=/work cgr.dev/chainguard/melange build melange.yaml --arch x86_64 --signing-key weather-api.rsa
```

#### 3. Bundle that APK with Wolfi into a file
```
docker run --privileged --volume "$PWD":/work --workdir=/work cgr.dev/chainguard/apko build apko.yaml weather-api weather-api.tar --arch x86_64
```

#### 4. Load the resulting image into Docker
```
docker load < weather-api.tar
```

#### 5. Run the application
```
docker run weather-api:latest-amd64
```