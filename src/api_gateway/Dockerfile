FROM python:3.13-slim

RUN apt-get update
WORKDIR /usr/src/app
ADD api_gateway/requirements.txt ./
RUN pip install -r requirements.txt

ADD api_gateway/context.py ./
ADD api_gateway/main.py ./

ADD api_gateway/api ./api


ADD api_gateway/lib ./lib

COPY services/account/proto/ ./proto/
RUN python -m grpc_tools.protoc -Igrpc_build=./proto --python_out=./ --pyi_out=./ --grpc_python_out=./ ./proto/account_service.proto

COPY services/user/proto/ ./proto/
RUN python -m grpc_tools.protoc -Igrpc_build=./proto --python_out=./ --pyi_out=./ --grpc_python_out=./ ./proto/user_service.proto

COPY services/cargo/proto/ ./proto/
RUN python -m grpc_tools.protoc -Igrpc_build=./proto --python_out=./ --pyi_out=./ --grpc_python_out=./ ./proto/cargo_service.proto

COPY services/delivery/proto/ ./proto/
RUN python -m grpc_tools.protoc -Igrpc_build=./proto --python_out=./ --pyi_out=./ --grpc_python_out=./ ./proto/delivery_service.proto

COPY services/payment/proto/ ./proto/
RUN python -m grpc_tools.protoc -Igrpc_build=./proto --python_out=./ --pyi_out=./ --grpc_python_out=./ ./proto/payment_service.proto

EXPOSE ${API_PORT}

ENTRYPOINT [ "uvicorn" , "main:app"]