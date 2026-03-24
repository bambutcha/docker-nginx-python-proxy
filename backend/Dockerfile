FROM python:3.12-slim-bookworm

WORKDIR /app

COPY app.py .

EXPOSE 8080

USER nobody

CMD ["python", "-u", "app.py"]
