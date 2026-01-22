# -------- Build stage --------
FROM mcr.microsoft.com/dotnet/sdk:6.0-jammy AS builder

WORKDIR /src

# Install build dependencies
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
      build-essential \
      libssl-dev \
      pkg-config \
      libboost-all-dev \
      libsodium-dev \
      libzmq5 \
      libzmq3-dev \
      ca-certificates \
      curl \
      git && \
    rm -rf /var/lib/apt/lists/*

# Miningcore source is provided as build context
COPY . .

# Apply LCC Miningcore patch
COPY patches /patches
RUN git apply /patches/lcc-actual-difficulty.patch

# Build Miningcore
WORKDIR /src/src/Miningcore
RUN dotnet publish -c Release --framework net6.0 -o /out -p:IsLinux=false


# -------- Runtime stage --------
FROM mcr.microsoft.com/dotnet/aspnet:6.0-jammy

WORKDIR /app

RUN apt-get update && apt-get -y install --no-install-recommends \
  libzmq5 \
  ca-certificates \
  curl \
  && (apt-get -y install --no-install-recommends libsodium23 || apt-get -y install --no-install-recommends libsodium-dev) \
  && rm -rf /var/lib/apt/lists/*

COPY --from=builder /out/ ./

ENV ASPNETCORE_URLS=http://0.0.0.0:4000
ENV LD_LIBRARY_PATH=/app

EXPOSE 4000 3333

CMD ["./Miningcore"]
