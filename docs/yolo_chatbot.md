# Construction Defects Detection Chatbot

## Overview

The Construction Defects Detection Chatbot is a `YOLO11` model designed to identify construction defects from images. The model categorizes defects into three classes:

- broken-brick

- drip-mortar

- lintel-damage

## Installation

### Prerequisites

- Python 3.10+

- [**uv**](https://astral.sh/uv/) installed.

### Steps to Install

Create and activate a virtual environment:

```sh
uv venv
source .venv/Scripts/activate
```

Install the required dependencies:

```sh
uv pip install -r requirements.txt
```


## Set up

### Frontend

Open `index.html` with Live Server on port 5500.

### Backend

Run the FastAPI development server with the following command:

``` sh
fastapi dev app.py
```