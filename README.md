# SVG Animation Parser & Editor

## Overview
This is a front-end web application that allows users to extract, edit, and reapply animations from SVG files using JSON.

## Features
✅ **Upload an SVG** – Users can upload an SVG file with CSS animations.
✅ **Extract & Parse Animations** – The app identifies all CSS animations and converts them into a structured JSON format.
✅ **Display SVG & JSON** – Both the animated SVG and its corresponding JSON representation are shown side by side.
✅ **Download JSON** – Users can download the extracted JSON without requiring a backend.
✅ **Upload JSON** – A previously exported JSON file can be uploaded to modify animations.
✅ **Apply JSON to SVG** – If the uploaded SVG matches the structure of the exported one, animations are reapplied.

## Run in development
### Installation
  ```sh
    git clone https://github.com/your-username/svg-animation-parser.git
    cd svg-animation-parser
  ```
### Project Setup
  ```sh
  npm install
  ```
### Compile and Hot-Reload for Development
```sh
npm run dev
```