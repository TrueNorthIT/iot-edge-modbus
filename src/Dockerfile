FROM microsoft/dotnet:2.0-sdk AS build-env
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.cs* ./
RUN dotnet restore

# build
RUN dotnet build
RUN dotnet publish -f netcoreapp2.0 -c Release -o out

FROM gcc:7 AS build-env-2
WORKDIR /app

# copy .c and .h file
COPY *.c ./
COPY *.h ./

# build
RUN gcc -shared -o libcomWrapper.so -fPIC comWrapper.c

FROM microsoft/dotnet:2.0.0-runtime

WORKDIR /app

COPY --from=build-env /app/out ./
COPY --from=build-env-2 /app/libcomWrapper.so /usr/lib/

CMD ["/usr/bin/dotnet", "iot-edge-modbus.dll"]