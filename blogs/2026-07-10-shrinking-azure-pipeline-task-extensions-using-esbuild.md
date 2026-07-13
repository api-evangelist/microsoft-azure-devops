---
title: "Shrinking Azure Pipeline task extensions using esbuild"
url: "https://devblogs.microsoft.com/devops/shrinking-azure-pipeline-task-extensions-using-esbuild/"
date: "2026-07-10"
author: "David Paquette"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
TL;DR We bundled an internal Azure Pipelines task extension into a single bundled JavaScript file using esbuild. The task package dropped from tens of megabytes and thousands of files to three files per task ( script.js , task.json , and icon.png ). The change took about 20 lines of build tooling.
