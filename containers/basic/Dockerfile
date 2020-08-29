
FROM python:3.8.5-slim AS build

RUN apt-get clean && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends \
    python3-dev build-essential ca-certificates

WORKDIR /build
		
COPY requirements.txt .
ENV PATH=/root/.local/bin:$PATH

RUN pip install --user --upgrade pip
RUN pip install --user cython
RUN pip install --user pyyaml
RUN pip install --user -r requirements.txt

COPY config.yml .

FROM python:3.8.5-slim 

RUN apt-get clean && \
    apt-get update -y && \
    apt-get install -y ca-certificates

ENV PATH="/opt/ml/code:/root/.local/bin:${PATH}"
RUN mkdir -p /opt/ml/code
WORKDIR /opt/ml/code

COPY --from=build /root/.local /root/.local
COPY --from=build /build/ .

ENTRYPOINT ["python3"]
