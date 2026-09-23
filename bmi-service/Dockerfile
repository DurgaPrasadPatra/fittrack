# Base image: official Python 3.12 on a slim Debian.
# "slim" removes extras like compilers and docs, so the image is much
# smaller than the full python:3.12 image (~150 MB vs ~1 GB).
FROM python:3.12-slim

# Sets the working directory inside the container.
# Creates /code if it doesn't exist, and every later instruction
# (COPY, RUN, CMD) runs relative to this path.
WORKDIR /code

# Copy ONLY the dependency list first.
# Docker caches each layer, so if requirements.txt hasn't changed,
# the pip install layer below is reused and doesn't run again.
COPY requirements.txt .

# Install the Python dependencies inside the image.
# --no-cache-dir stops pip from storing its download cache in the layer,
# which keeps the image smaller.
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application code AFTER installing dependencies.
# Code changes often, so keeping this layer last means edits to main.py
# only rebuild this layer and not the dependency install.
COPY app ./app

# Documents that the app listens on port 8000.
# This is metadata only and does NOT publish the port.
# You still need -p 8000:8000 on docker run (or ports: in Compose).
EXPOSE 8000

# The default command when the container starts.
# - Exec form (JSON array) makes uvicorn PID 1, so it receives
#   stop signals (SIGTERM) properly and shuts down cleanly.
# - --host 0.0.0.0 makes the server listen on all interfaces.
#   With the default 127.0.0.1, it would be unreachable from outside
#   the container, even with -p.
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
