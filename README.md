# gitlab-ci-android-ndk

This Docker image contains the Android SDK and most common packages necessary
for building Android apps in a CI tool like GitLab CI. Make sure your CI
environment's caching works as expected, this greatly improves the build time,
especially if you use multiple build jobs.

*Note:* This was originally forked from [jangrewe/gitlab-ci-android](https://github.com/jangrewe/gitlab-ci-android),
        but has by now diverged.

- It has a full suite of native build tools thanks to being based on
  [jfinkhaeuser/gitlab-ci-cpp-meson](https://github.com/jfinkhaeuser/gitlab-ci-cpp-meson)
- It also has `ndk-bundle` installed.

A `.gitlab-ci.yml` with caching of your project's dependencies would look like this:

```
image: jfinkhaeuser/gitlab-ci-android-ndk

stages:
- build

before_script:
- export GRADLE_USER_HOME=$(pwd)/.gradle
- chmod +x ./gradlew

cache:
  key: ${CI_PROJECT_ID}
  paths:
  - .gradle/

build:
  stage: build
  script:
  - ./gradlew assembleDebug
  artifacts:
    paths:
    - app/build/outputs/apk/app-debug.apk
```
