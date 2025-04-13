# Folosește o imagine oficială Python
FROM python:3.9-slim

# Setează directorul de lucru
WORKDIR /app

# Copiază fișierul requirements.txt și instalează dependențele
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Copiază tot codul aplicației
COPY . .

# Expune portul pe care rulează FastAPI (aici, 8000)
EXPOSE 8000

# Comanda care rulează serverul FastAPI
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
