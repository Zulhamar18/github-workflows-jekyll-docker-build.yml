# github-workflows-jekyll-docker-build.yml
name: Build Jekyll site using Docker

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # Langkah untuk checkout repository
      - name: Checkout code
        uses: actions/checkout@v2

      # Langkah untuk setup Docker
      - name: Set up Docker
        uses: docker/setup-buildx-action@v2

      # Langkah untuk menarik Docker image Jekyll dan membangun situs
      - name: Build site with Jekyll Docker image
        run: |
          docker pull jekyll/builder:latest
          docker run --rm \
            -v ${{ github.workspace }}:/srv/jekyll \
            -w /srv/jekyll \
            jekyll/builder:latest jekyll build
          
      # Langkah untuk memverifikasi build selesai
      - name: Verify build
        run: |
          echo "Build selesai!"
