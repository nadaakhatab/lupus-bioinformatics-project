# Tools: SRA Toolkit · FastQC · Fastp · MultiQC

FROM ubuntu:22.04

LABEL maintainer="bioinfo-project"
LABEL description="RNA-seq pipeline for Lupus SLE analysis"
LABEL version="1.0"

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
ENV WORKDIR=/workspace
WORKDIR ${WORKDIR}
ENV PYTHONPATH=${WORKDIR}/src:${WORKDIR}

# Tool versions
ENV SRA_VERSION=3.1.1
ENV FASTQC_VERSION=0.12.1
ENV FASTP_VERSION=0.23.4

# 1. System packages
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    wget curl unzip bzip2 ca-certificates gnupg \
    default-jre-headless build-essential \
    zlib1g-dev libbz2-dev liblzma-dev \
    python3.11 python3.11-dev python3-pip perl \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python  python  /usr/bin/python3.11 1 \
 && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1 \
 && update-alternatives --install /usr/bin/pip     pip     /usr/bin/pip3       1

# 2. SRA Toolkit (Modified to use curl -L)
RUN curl -L "https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/${SRA_VERSION}/sratoolkit.${SRA_VERSION}-ubuntu64.tar.gz" \
    -o /tmp/sratoolkit.tar.gz \
 && tar -xzf /tmp/sratoolkit.tar.gz -C /opt \
 && mv /opt/sratoolkit.${SRA_VERSION}-ubuntu64 /opt/sratoolkit \
 && rm /tmp/sratoolkit.tar.gz

ENV PATH="/opt/sratoolkit/bin:${PATH}"

RUN mkdir -p /root/.ncbi \
 && printf '/LIBS/GUID = "%s"\n' "bioinfo-docker-uuid" > /root/.ncbi/user-settings.mkfg \
 && printf '/repository/user/main/public/root = "%s"\n' "/workspace/data/.sra_cache" >> /root/.ncbi/user-settings.mkfg

# 3. FastQC (Modified to use curl -L)
RUN curl -L "https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v${FASTQC_VERSION}.zip" \
    -o /tmp/fastqc.zip \
 && unzip -q /tmp/fastqc.zip -d /opt \
 && chmod +x /opt/FastQC/fastqc \
 && ln -s /opt/FastQC/fastqc /usr/local/bin/fastqc \
 && rm /tmp/fastqc.zip

# 4. Fastp 
RUN curl -L "https://github.com/OpenGene/fastp/releases/download/v${FASTP_VERSION}/fastp" \
    -o /usr/local/bin/fastp \
 && chmod +x /usr/local/bin/fastp

# 5. Python dependencies
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir --upgrade pip setuptools wheel \
 && pip install --no-cache-dir -r /tmp/requirements.txt

# 6. Directory structure
RUN mkdir -p ${WORKDIR}/data/healthy ${WORKDIR}/data/disease ${WORKDIR}/data/.sra_cache \
    ${WORKDIR}/src ${WORKDIR}/results/fastqc_raw ${WORKDIR}/results/fastqc_trimmed \
    ${WORKDIR}/results/fastp ${WORKDIR}/results/multiqc ${WORKDIR}/reports

# 7. Verification
RUN echo "=== Checking Tools ===" \
 && python --version && fastqc --version && fastp --version 2>&1 | head -1 \
 && multiqc --version

CMD ["/bin/bash"]