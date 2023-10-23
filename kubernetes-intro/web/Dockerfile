FROM python:3-slim
RUN useradd -u 1001 myuser
USER myuser
WORKDIR /app
EXPOSE 8000
CMD ["python", "-m", "http.server", "-d", "/app", "8000"]