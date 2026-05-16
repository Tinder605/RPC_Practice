FROM python:3.12-slim

WORKDIR /app

COPY ./server/requirements.txt .
RUN pip install -r requirements.txt

COPY ./server .
COPY ./proto ./proto

RUN python -m grpc_tools.protoc \
    -I./proto \
    --python_out=. \
    --grpc_python_out=. \
    ./proto/calculator.proto

CMD ["python", "server.py"]