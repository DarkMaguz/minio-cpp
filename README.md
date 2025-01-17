> NOTE: This project is work in progress.

# MinIO C++ Client SDK for Amazon S3 Compatible Cloud Storage [![Slack](https://slack.min.io/slack?type=svg)](https://slack.min.io) [![Sourcegraph](https://sourcegraph.com/github.com/minio/minio-cpp/-/badge.svg)](https://sourcegraph.com/github.com/minio/minio-cpp?badge) [![Apache V2 License](https://img.shields.io/badge/license-Apache%20V2-blue.svg)](https://github.com/minio/minio-cpp/blob/master/LICENSE)

The MinIO C++ Client SDK provides simple APIs to access any Amazon S3 compatible object storage. This quickstart guide will show you how to install the MinIO client SDK, connect to MinIO, and provide a walkthrough for a simple file uploader. For a complete list of APIs and examples, please take a look at the [MinIO C++ Client API Reference](https://minio-cpp.min.io/)

This document assumes that you have a working C++ development environment. In order to build this project, you need the Cross-Platform Make CMake 3.10 or higher, [vcpkg](https://vcpkg.io/en/index.html).

## Install from `vcpkg`
```
vcpkg install minio-cpp
```

## Source build

```
git clone https://github.com/minio/minio-cpp; cd minio-cpp;
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=${VCPKGDIR}/scripts/buildsystems/vcpkg.cmake
cmake --build build
```

## Example code
```c++
#include <iostream>
#include <fstream>
#include <stdlib.h>
#include <getopt.h>
#include <s3.h>

using namespace Minio;

int
main ( int argc, char** argv )
{
  S3Client s3("https://play.min.io:9000", "minioadmin", "minioadmin");
  S3ClientIO io;
  s3.MakeBucket("newbucket", io);
  if(io.Failure()) {
    std::cerr << "ERROR: failed to create bucket" << endl;
    std::cerr << "response:\n" << io << endl;
    std::cerr << "response body:\n" << io.response.str() << endl;
    return -1;
  }
  return 0;
}
```

## Run an example
Following example runs 'multipart' upload, uploads a single part. You would have to choose a local file to upload for `-f`, and also remote bucket to upload the object to as `-n` and final object name in the bucket as `-k`.

```
export ACTION="multipart"
export ACCESS_KEY=minioadmin
export SECRET_KEY=minioadmin
export ENDPOINT="https://play.min.io:9000"

./examples/s3 -a ${ACTION} -f <local_filename_to_upload> \
              -n <remote_bucket> -k <remote_objectname>
```

Please choose a `<remote_bucket>` that exists.

## License
This SDK is distributed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0), see [LICENSE](https://github.com/minio/minio-cpp/blob/master/LICENSE) for more information.
