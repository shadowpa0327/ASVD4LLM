# ASVD: Activation-aware Singular Value Decomposition for Compressing Large Language Models

This repo is the modified version of ASVD for porting into Atom

# Requirement
- python>=3.10
- pip install -r requirements.txt

# Run ASVD

## Step-1 Get the cache file of calibration
You can use the cache file to omit the calibration process. The cache file can be downloaded from huggingface-hub, by using the following command:
```
git clone https://huggingface.co/hahnyuan/ASVD4LLM_sensitivity_cache cache
```
Or download the cache file from [here](https://huggingface.co/hahnyuan/ASVD4LLM_sensitivity_cache/tree/main) yourself. And place the cache file in the `cache` folder.

## Step-2 Making huggingface repository for later usage

You can use the following command to make a huggingface repository for your ASVD model. 


Examples:
```
CUDA_VISIBLE_DEVICES='0' python huggingface_repos/build_asvd_repo.py \
--model_id="meta-llama/Llama-2-7b-hf" \
--act_aware \
--alpha 0.5 \
--n_calib_samples 32 \
--scaling_method abs_mean \
--param_ratio_target 0.8 \
--use_cache
```

After running this commands, you will see a directory be generated in the `huggingface_repos/Llama-2-7b-hf-asvd80` which contain the low-rank llama-2 models.





