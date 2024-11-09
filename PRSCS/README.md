# tools
This Dockerfile creates a container with the following tools:  
•PRSCS (v1.1.0. downloaded on Nov 9, 2024)  
Note that scipy and h5py are needed for PRSCS.
•plink1.9 (plink_linux_x86_64_20241022)  
•plink2 (plink2_linux_x86_64_20241020)  
•bgenix  
  
Note that the image is for amd64/linux_x86_64 and not for amd64 (Mac etc).  

# build and push  
`docker buildx build --platform linux/amd64 -t sysgits/prscs:latest --push .  `
  
# pull the image from HPC/VM  
`module load apptainer/1.2 && apptainer pull docker://sysgits/prscs:latest`  
`/root/miniforge3/envs/test/bin/python PRScs.py`
