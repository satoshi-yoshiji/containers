# Use Ubuntu 20.04 as the base image
FROM ubuntu:20.04

# Define non-interactive frontend and set HOME environment variable
ENV DEBIAN_FRONTEND=noninteractive
ENV HOME=/root
ENV PATH="$HOME/miniforge3/bin:$HOME/local/bin:$PATH"

WORKDIR /root

# Update and install essential packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        python3-dev \
        cmake \
        git \
        wget \
        unzip \
        openjdk-11-jre-headless \
        ca-certificates \
        libglib2.0-0 \
        libxext6 \
        libsm6 \
        libxrender1 \
        && \
    # Clean up APT cache to reduce image size
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Miniforge for amd64
RUN wget -qO /tmp/miniforge.sh https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh && \
    bash /tmp/miniforge.sh -b -p $HOME/miniforge3 && \
    rm /tmp/miniforge.sh && \
    # Initialize conda for bash
    $HOME/miniforge3/bin/conda init bash

# Create and configure the 'test' Conda environment
RUN /bin/bash -c "source $HOME/miniforge3/bin/activate && \
    conda create -n test python=3.7 -y && \
    conda activate test && \
    conda install -c bioconda -y tabix bedtools && \
    conda install -c conda-forge -y bgenix scipy h5py && \
    conda config --env --add channels bioconda && \
    conda config --env --add channels conda-forge && \
    conda install -n test -y samtools=1.11 && \
    conda clean -afy"

# Clone PRScs repository
RUN git clone https://github.com/getian107/PRScs.git $HOME/PRScs

# Install bioinformatics tools
RUN mkdir -p $HOME/local/bin && \
    cd $HOME/local/bin && \
    wget https://s3.amazonaws.com/plink2-assets/plink2_linux_x86_64_20241020.zip && \
    unzip plink2_linux_x86_64_20241020.zip && \
    wget https://s3.amazonaws.com/plink1-assets/plink_linux_x86_64_20241022.zip && \
    unzip plink_linux_x86_64_20241022.zip && \
    wget https://github.com/broadinstitute/gatk/releases/download/4.2.3.0/gatk-4.2.3.0.zip && \
    unzip gatk-4.2.3.0.zip && \
    ln -sn gatk-4.2.3.0/gatk-package-4.2.3.0-local.jar gatk && \
    wget https://github.com/broadinstitute/picard/releases/download/2.26.4/picard.jar && \
    rm *.zip

# Set the PATH to include the conda environment's binaries
ENV PATH="$HOME/miniforge3/envs/test/bin:$HOME/local/bin:$PATH"

# Optional: Activate the 'test' environment by default
RUN echo "conda activate test" >> ~/.bashrc

# Default command
CMD ["/bin/bash"]
