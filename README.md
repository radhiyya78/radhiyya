# This workflow will build a golang project
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-go

name: Go

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:

  build:  
  runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Set up Go
      uses: actions/setup-go@v2
      with:
        go-version: '1.20'
        - name: Build
      run: go build -v ./...

    - name: Login
      run: go test -v ./...

    - name: Login to Docker Hub
      uses: docker/login-action@v3
      with:
        username: ${{ vars.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    - name: Build and push
      uses: docker/build-push-action@v6
      with:
        push: true
        tags: radhiyyap/radhiyya78:latest
