Images

```
singularity oci run docker://svlprhpcreg01.stjude.org/hpcf/chipseq-analysis:latest
singularity oci run docker://svlprhpcreg01.stjude.org/hpcf/chipseq:latest
singularity oci run docker://svlprhpcreg01.stjude.org/hpcf/ripseq-analysis:latest
```

Problems encountered:
initialization of pipeline is broken, the demo files aren't being generated from the git repo. I'm not certain what the objective is, if it is to run the demo, I am at a dead end, if it is to fit one's own dataset onto it, I hope these images work. 

Building on cnvrg

could not build on cnvrg directly, modified the files to build to avoid docker mounts
pipeline and analysis workflows rely on this binding.  modified with cp, which is what I attempted to do and that's what the containers contain. 

Changes / deviations from the git repo for my build process

# Rip-seq
make sure
```
apt-get install default-jre
apt-get install graphviz
```

change docker file from conda to mamba, as conda freezes during solve

```dockerfile
FROM mambaorg/micromamba

RUN apt-get update && apt-get install wget

RUN micromamba activate && micromamba install python=3.7 jupyter -c conda-forge && \
    micromamba activate base && \
    micromamba config append channels conda-forge && \
    micromamba config append channels bioconda && \
    micromamba install -y -c bioconda \
                            r-base=4.1.1 \
                            bioconductor-deseq2=1.32.0 \
                            bioconductor-apeglm=1.14.0 \
                            bioconductor-tximport=1.20.0 \
                            bioconductor-degreport=1.28.0 \
                            pybedtools=0.8.2 \
                            deeptools=3.5.1 \
                            seaborn=0.11.2 \
                            pandas=1.2.4 \
                            matplotlib=3.4.1 \
                            joblib=0.14.1 \
                            biopython=1.76

RUN python3 -m pip install htseq==0.12.3

WORKDIR /project
```

```bash
cd analysis
# sudo docker build -t ripseq-analysis:latest docker/

# build the image
docker run -it --rm -u 0 --name ripseq-build mambaorg/micromamba:jammy /bin/bash
micromamba activate && micromamba install python=3.7 jupyter -c -y conda-forge
micromamba config append channels conda-forge
micromamba config append channels bioconda
micromamba install -y -c bioconda \
							r-base=4.1.1 \
                            bioconductor-deseq2=1.32.0 \
                            bioconductor-apeglm=1.14.0 \
                            bioconductor-tximport=1.20.0 \
                            bioconductor-degreport=1.28.0 \
                            pybedtools=0.8.2 \
                            deeptools=3.5.1 \
                            seaborn=0.11.2 \
                            pandas=1.2.4 \
                            matplotlib=3.4.1 \
                            joblib=0.14.1 \
                            biopython=1.76
# quit and commit
CMD-P CMD-Q
sudo docker commit ripseq-build ripseq-analysis:latest
# run the container
# sudo docker run --rm -it -v $(pwd):/project --name ripseq-analysis-container ripseq-analysis:latest

# copy the files into the container
sudo docker run --rm -itd --name ripseq-analysis-container ripseq-analysis:latest
sudo docker cp ../analysis/. ripseq-analysis-container:/project
sudo docker exec --it -u 0 ripseq-analysis-container /bin/bash
# when finished copy it back out
sudo docker cp ripseq-analysis-container:/project ../analysis
```

# Chip-seq
remove versions from apt-get as those versions are no longer found
#### Pipeline
also changed to mamba
```dockerfile 
FROM mambaorg/micromamba:jammy

RUN apt-get update && apt-get install wget

RUN micromamba activate && micromamba install python=3.7 jupyter -c -y conda-forge && \
    micromamba activate base && \
    micromamba config append channels conda-forge && \
    micromamba config append channels bioconda && \
    micromamba install -y -c bioconda \
                            python=3.7 \
                            fastqc=0.11.9 \
                            samtools=1.12 \
                            macs2=2.2.6 \
                            minimap2=2.18 \
                            deeptools=3.5.1 \
                            snakemake-minimal=5.26.0 \
                            multiqc=1.10.1 \
                            bedtools=2.30.0 \
                            ucsc-bedclip=377 \
                            ucsc-bedgraphtobigwig=377 \
                            trim-galore=0.6.6

# samtools fix
RUN cd /opt/conda/lib && ln -s libcrypto.so.1.1 libcrypto.so.1.0.0

# fresh sratoolkit
WORKDIR /tools
RUN wget --output-document sratoolkit.tar.gz http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-ubuntu64.tar.gz && \
    tar -vxzf sratoolkit.tar.gz
ENV PATH "$PATH:/tools/sratoolkit.2.10.8-ubuntu64/bin"

WORKDIR /project
```

``` bash
cd pipeline
sudo docker build -t chipseq:latest docker/
# sudo docker run --rm -it -v $(pwd):/project --name chipseq-container chipseq:latest
sudo docker run --rm -itd --name chipseq-container chipseq:latest
sudo docker cp ../pipeline/. chipseq-container:/project

sudo docker attach chipseq-analysis-container
snakemake --cores $(nproc) --configfile config/config.yaml -d . --snakefile workflow/Snakefile  all


```
#### Analysis
```dockerfile
FROM ubuntu:20.04

# tzdata package is required for r-base
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Moscow

RUN apt-get update && apt-get install -y \
    python3.8 python3.8-dev python3-pip \
    bedtools=2.27.1+dfsg-4ubuntu1 \
    r-base=3.6.3-2 \
    libssl-dev \
    curl \
    libcurl4-openssl-dev \
    libxml2-dev \
    clustalo=1.2.4-4build1 \
    build-essential libtool-bin automake \
    && rm -rf /var/lib/apt/lists/*


RUN python3.8 -m pip install \
    pybedtools==0.8.2 \
    tqdm==4.60.0 \
    numpy==1.20.1 \
    matplotlib==3.1.3 \
    HTSeq==0.12.3 \
    rpy2==3.3.6 \
    pandas==1.1.5 \
    matplotlib_venn==0.11.6 \
    biopython==1.76 \
    parasail==1.2.2

# requires numpy installed
RUN python3.8 -m pip install \
    pyBigWig==0.3.17


WORKDIR /project
```

```bash
cd analysis
sudo docker build -t chipseq-analysis:latest docker/

#sudo docker run --rm -it -v $(pwd):/project --name chipseq-analysis-container chipseq-analysis:latest
# copy files into container
sudo docker run --rm -itd --name chipseq-analysis-container chipseq-analysis:latest
sudo docker cp ../analysis/. chipseq-analysis-container:/project

sudo docker attach chipseq-analysis-container
```
