FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="$HOME/local/bin/:${PATH}"

WORKDIR /root

# Install essential packages, Vim, and setup YouCompleteMe
RUN \
  apt update && \
  apt install -y build-essential python3-dev cmake git vim wget unzip openjdk-8-jre-headless && \
  mkdir -p $HOME/.vim/bundle && cd ~/.vim/bundle && \
  git clone https://github.com/VundleVim/Vundle.vim.git && \
  wget -O $HOME/.vimrc https://raw.githubusercontent.com/skoyamamd/gcp_dotfiles/master/vimrc && \
  vim -c PluginInstall -c qall && \
  cd $HOME/.vim/bundle/YouCompleteMe && \
  python3 install.py

# Install Miniconda, create a conda environment, install bioinformatics tools, scipy, h5py, and clone PRScs
RUN \
  cd $HOME && \
  wget --no-check-certificate https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
  bash ~/miniconda.sh -b -p $HOME/miniconda && \
  rm ~/miniconda.sh && \
  eval "$($HOME/miniconda/bin/conda shell.bash hook)" && \
  conda init && \
  echo "conda activate test" >> ~/.bashrc && \
  conda create -n test python=3.7 -y && \
  conda activate test && \
  conda install -c bioconda -y tabix bedtools && \
  conda install -c conda-forge -y bgenix && \
  conda config --add channels bioconda && \
  conda config --add channels conda-forge && \
  conda install -y samtools=1.11 scipy h5py && \
  # Clone PRScs repository
  git clone https://github.com/getian107/PRScs.git $HOME/PRScs

# Install bioinformatics tools
RUN \
  mkdir -p $HOME/local/bin && \
  cd $HOME/local/bin && \
  wget https://s3.amazonaws.com/plink2-assets/plink2_linux_x86_64_20211011.zip && \
  unzip plink2_linux_x86_64_20211011.zip && \
  wget https://s3.amazonaws.com/plink1-assets/dev/plink_linux_x86_64.zip && \
  unzip plink_linux_x86_64.zip && \
  wget https://github.com/broadinstitute/gatk/releases/download/4.2.3.0/gatk-4.2.3.0.zip && \
  unzip gatk-4.2.3.0.zip && \
  ln -sn gatk-4.2.3.0/gatk-package-4.2.3.0-local.jar gatk && \
  wget https://github.com/broadinstitute/picard/releases/download/2.26.4/picard.jar && \
  rm *.zip

# (Optional) Set the default conda environment
ENV PATH="$HOME/miniconda/envs/test/bin:$PATH"
