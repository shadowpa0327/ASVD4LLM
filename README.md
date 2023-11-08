# Run
CUDA_VISIBLE_DEVICES='0,1' python svd_lora_train.py --model_id="huggyllama/llama-7b" --rank_compress_ratio=0.2 --lora_method "Uonly"
CUDA_VISIBLE_DEVICES='2,3' python svd_lora_train.py --model_id="huggyllama/llama-7b" --rank_compress_ratio=0.2 --lora_method "Vonly"
CUDA_VISIBLE_DEVICES='2' python svd_lora_train.py --model_id="facebook/opt-125m" --lora_method "Uonly" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='0' python svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "Uonly" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='1' python svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "Vonly" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='2' python svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "UV" --rank_compress_ratio=0.2

CUDA_VISIBLE_DEVICES='0' python act_aware_svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "Uonly" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='1' python act_aware_svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "Vonly" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='2' python act_aware_svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "UV" --rank_compress_ratio=0.2
CUDA_VISIBLE_DEVICES='3' python act_aware_svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "UVall" --rank_compress_ratio=0.2

CUDA_VISIBLE_DEVICES='2' python act_aware_svd_lora_train.py --model_id="facebook/opt-125m" --lora_method "Uonly" --rank_compress_ratio=0.2

CUDA_VISIBLE_DEVICES='2' python svd_lora_train.py --model_id="facebook/opt-1.3b" --lora_method "reconstruct"

# Eval
CUDA_VISIBLE_DEVICES='0' python tools/eval_checkpoint.py "huggyllama/llama-7b" output/svd_lora_train/checkpoint-2000 --rank_compress_ratio=0.2 --lora_method "UV"
CUDA_VISIBLE_DEVICES='0' python tools/eval_checkpoint.py "huggyllama/llama-7b" output/svd_lora_train_Uonly_0.2/checkpoint-2000 --rank_compress_ratio=0.2 --lor`a_method "Uonly" --limit 200
CUDA_VISIBLE_DEVICES='1' python tools/eval_checkpoint.py --model_id="huggyllama/llama-7b" --path=output/svd_lora_train_Vonly_0.2/checkpoint-2000 --rank_compress_ratio=0.2 --lora_method "Vonly"

CUDA_VISIBLE_DEVICES='2' python tools/eval_checkpoint.py --model_id="facebook/opt-1.3b" --lora_method "UV" --rank_compress_ratio=0.2 --path="output/facebook_opt-1.3b_svd_lora_train_UV_0.2/checkpoint-3000"
CUDA_VISIBLE_DEVICES='1' python tools/eval_checkpoint.py --model_id="facebook/opt-1.3b" --lora_method "Uonly" --rank_compress_ratio=0.2 --path="output/facebook_opt-1.3b_svd_lora_train_Uonly_0.2/checkpoint-3000"
CUDA_VISIBLE_DEVICES='0' python tools/eval_checkpoint.py --model_id="facebook/opt-1.3b" --lora_method "Vonly" --rank_compress_ratio=0.2 --path="output/facebook_opt-1.3b_svd_lora_train_Vonly_0.2/checkpoint-3000"


CUDA_VISIBLE_DEVICES='0' python tools/eval_checkpoint.py --model_id="facebook/opt-1.3b" --lora_method "Uonly" --rank_compress_ratio=0.2 --path="output/facebook_opt-1.3b_act_aware_Uonly_0.2/checkpoint-3000"

output/facebook_opt-1.3b_svd_lora_train_reconstruct_0.3_0.1/checkpoint-3000

CUDA_VISIBLE_DEVICES='2' python tools/eval_checkpoint.py --model_id="facebook/opt-1.3b" --lora_method "reconstruct" --path="output/facebook_opt-1.3b_mixed_rank_UV_14.8_True"


CUDA_VISIBLE_DEVICES='3' python tools/eval_checkpoint.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --path="output/facebook_opt-125m_mixed_rank_reconstruct_29.0_True/final_merged"



# Sensitivity
CUDA_VISIBLE_DEVICES='0' python experiments/sensitivity.py --model_id="facebook/opt-125m" --lora_method "reconstruct"

CUDA_VISIBLE_DEVICES='2' python experiments/sensitivity.py --model_id="facebook/opt-1.3b" --lora_method "reconstruct"

CUDA_VISIBLE_DEVICES='3' python experiments/sensitivity.py --model_id="facebook/opt-1.3b" --lora_method "reconstruct" --act_aware

CUDA_VISIBLE_DEVICES='2' python experiments/sensitivity.py --model_id="huggyllama/llama-7b" --lora_method "reconstruct"

CUDA_VISIBLE_DEVICES='3' python experiments/sensitivity.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware

CUDA_VISIBLE_DEVICES='3' python experiments/sensitivity.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware

CUDA_VISIBLE_DEVICES='0' python experiments/sensitivity.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware

# Mixed rank
CUDA_VISIBLE_DEVICES='3' python mixed_rank_svd_lora_train.py --model_id="facebook/opt-125m" --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --ppl_thresh 29 --act_aware --lora_method "reconstruct"

CUDA_VISIBLE_DEVICES='2' python mixed_rank_svd_lora_train.py --model_id="facebook/opt-1.3b" --sensitivity_json output/sensitivity_facebook_opt-1.3b_False.json --ppl_thresh 14.8 --act_aware --lora_method "reconstruct"

CUDA_VISIBLE_DEVICES='0' python mixed_rank_svd_lora_train.py --model_id="facebook/opt-125m" --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --ppl_thresh 29 --act_aware --lora_method "UV"

CUDA_VISIBLE_DEVICES='3' python mixed_rank_svd_lora_train.py --model_id="facebook/opt-1.3b" --sensitivity_json output/sensitivity_facebook_opt-1.3b_False.json --ppl_thresh 14.8 --act_aware --lora_method "UV"

# single block sensitivity
CUDA_VISIBLE_DEVICES='0' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name model.decoder.layers.0. --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 28 --act_aware

CUDA_VISIBLE_DEVICES='1' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name model.decoder.layers.0. --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 28.5 --act_aware

CUDA_VISIBLE_DEVICES='2' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name model.decoder.layers.0. --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 29 --act_aware

CUDA_VISIBLE_DEVICES='0' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name v_proj --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 28 --act_aware

CUDA_VISIBLE_DEVICES='1' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name v_proj --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 28.5 --act_aware

CUDA_VISIBLE_DEVICES='2' python experiments/single_block_sensitivity.py --model_id="facebook/opt-125m" --layer_name v_proj --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --ppl_thresh 29 --act_aware

# sequential svd rank search


CUDA_VISIBLE_DEVICES='0' python experiments/sequential_svd_rank_search.py --model_id="facebook/opt-125m" --layer_name model.decoder.layers.5. --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --act_aware

CUDA_VISIBLE_DEVICES='3' python experiments/sequential_svd_rank_search.py --model_id="facebook/opt-125m" --layer_name model.decoder.layers.9. --sensitivity_json output/sensitivity_facebook_opt-125m_True.json --lora_method "reconstruct" --act_aware

# Greedy
CUDA_VISIBLE_DEVICES='0' python experiments/greedy.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware --ppl_target_st 29 --ppl_target_ed 40

CUDA_VISIBLE_DEVICES='2' python experiments/greedy.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware --ppl_target_st 30 --ppl_target_ed 35

CUDA_VISIBLE_DEVICES='3' python experiments/greedy.py --model_id="facebook/opt-1.3b" --lora_method "reconstruct" --act_aware --ppl_target_st 15.1 --ppl_target_ed 20

CUDA_VISIBLE_DEVICES='3' python experiments/greedy.py --model_id="huggyllama/llama-7b" --lora_method "reconstruct" --act_aware --ppl_target_st 5.8 --ppl_target_ed 8

# Eval after greedy

CUDA_VISIBLE_DEVICES='2' python experiments/viz_after_greedy.py --model_id="facebook/opt-125m" --lora_method "UV" --act_aware --ratios "[1, 1, 0.8, 0.4, 1, 0.8, 1, 1, 0.8, 0.8, 1, 0.6, 1, 0.8, 1, 1, 1, 0.6, 1, 1, 0.8, 1, 1, 0.6, 0.8, 1, 0.8, 0.6, 1, 0.6, 0.8, 1, 1, 0.4, 1, 0.4, 0.6, 1, 1, 0.8, 1, 0.6, 1, 1, 0.8, 0.6, 0.8, 1, 1, 1, 0.6, 0.6, 1, 0.8, 0.8, 1, 0.8, 0.6, 1, 0.8, 0.8, 1, 1, 0.4, 0.6, 0.4, 0.4, 1, 1, 0.6, 0.8, 1, 0.8]"

CUDA_VISIBLE_DEVICES='2' python experiments/viz_after_greedy.py --model_id="facebook/opt-1.3b" --lora_method "UV" --act_aware --ratios "[1, 1, 0.4, 0.4, 0.8, 0.4, 0.4, 1, 1, 0.6, 1, 0.4, 0.4, 1, 0.8, 0.8, 0.8, 1, 0.6, 0.8, 0.6, 1, 0.8, 0.4, 0.8, 0.6, 0.4, 0.4, 1, 0.2, 0.4, 0.4, 0.4, 0.8, 1, 0.6, 0.6, 0.8, 1, 0.2, 0.8, 0.4, 0.6, 1, 1, 0.6, 0.6, 0.8, 1, 0.6, 1, 0.8, 0.8, 0.6, 0.8, 1, 0.8, 0.6, 1, 1, 1, 1, 0.6, 0.2, 0.8, 0.4, 1, 1, 0.6, 1, 0.6, 1, 0.6, 0.8, 1, 0.8, 0.8, 0.6, 0.4, 1, 0.8, 0.4, 0.8, 0.8, 1, 1, 0.8, 0.2, 0.8, 0.1, 1, 1, 1, 0.4, 1, 0.4, 0.4, 1, 0.8, 0.6, 0.6, 0.4, 0.8, 1, 0.8, 0.4, 0.6, 0.4, 0.6, 0.8, 0.6, 0.8, 0.4, 1, 0.1, 1, 0.8, 0.4, 0.4, 0.4, 0.6, 0.8, 0.8, 0.4, 0.2, 0.4, 0.2, 0.6, 1, 0.2, 0.1, 0.4, 0.1, 1, 0.8, 0.1, 0.2, 0.1, 0.2, 1, 1, 0.1, 0.4, 0.2, 0.4]"
CUDA_VISIBLE_DEVICES='1' python experiments/viz_after_greedy.py --model_id="facebook/opt-1.3b" --lora_method "UV" --act_aware --ratios "[1, 1, 0.8, 0.4, 1, 0.4, 0.4, 1, 1, 0.8, 1, 0.8, 0.8, 1, 0.8, 1, 0.8, 1, 1, 0.8, 0.6, 1, 0.8, 0.6, 1, 0.8, 0.6, 0.4, 1, 0.6, 0.6, 0.2, 1, 1, 1, 0.4, 0.8, 1, 1, 0.4, 1, 0.2, 0.8, 1, 1, 0.6, 1, 0.8, 1, 0.6, 1, 0.6, 0.8, 0.6, 0.8, 1, 1, 0.6, 1, 0.6, 1, 1, 0.6, 0.8, 1, 0.2, 0.6, 1, 0.8, 0.6, 0.8, 0.6, 0.6, 1, 1, 0.6, 0.8, 1, 0.8, 1, 0.8, 0.8, 0.8, 0.6, 1, 1, 0.8, 0.6, 0.8, 0.6, 0.4, 1, 1, 0.4, 1, 0.4, 1, 1, 0.8, 0.6, 1, 0.6, 0.4, 0.6, 0.8, 0.4, 1, 0.4, 0.4, 0.8, 0.6, 0.4, 0.1, 0.1, 0.1, 0.6, 0.8, 0.6, 0.4, 0.8, 0.1, 0.8, 1, 0.4, 0.6, 0.4, 0.2, 0.6, 1, 0.2, 0.2, 0.4, 0.2, 0.8, 0.8, 0.1, 0.4, 0.4, 0.2, 1, 1, 0.1, 0.8, 0.4, 0.4]"

CUDA_VISIBLE_DEVICES='1' python experiments/viz_after_greedy.py --model_id="huggyllama/llama-7b" --lora_method "UV" --act_aware --ratios "[1, 0.6, 1, 0.6, 0.1, 0.1, 0.2, 0.1, 1, 1, 0.8, 0.1, 0.1, 0.6, 0.4, 1, 1, 0.8, 0.1, 0.1, 0.8, 1, 1, 1, 0.8, 0.4, 0.4, 1, 0.4, 0.8, 1, 0.8, 0.1, 0.2, 0.8, 0.6, 1, 0.8, 1, 0.4, 0.6, 0.6, 0.6, 0.8, 0.8, 0.8, 0.6, 0.6, 1, 0.2, 1, 1, 0.8, 0.1, 0.6, 0.4, 1, 1, 0.8, 1, 0.2, 0.6, 0.4, 0.2, 1, 1, 0.6, 0.6, 0.6, 0.8, 0.6, 0.8, 1, 1, 0.4, 0.4, 0.4, 0.8, 1, 1, 0.8, 0.1, 0.6, 0.6, 1, 1, 1, 1, 0.1, 0.1, 0.6, 1]"

CUDA_VISIBLE_DEVICES='2' python experiments/test_after_greedy.py --model_id="facebook/opt-125m" --lora_method "UV" --act_aware --ratios "[1, 1, 0.8, 0.4, 1, 0.8, 1, 1, 0.8, 0.8, 1, 0.6, 1, 0.8, 1, 1, 1, 0.6, 1, 1, 0.8, 1, 1, 0.6, 0.8, 1, 0.8, 0.6, 1, 0.6, 0.8, 1, 1, 0.4, 1, 0.4, 0.6, 1, 1, 0.8, 1, 0.6, 1, 1, 0.8, 0.6, 0.8, 1, 1, 1, 0.6, 0.6, 1, 0.8, 0.8, 1, 0.8, 0.6, 1, 0.8, 0.8, 1, 1, 0.4, 0.6, 0.4, 0.4, 1, 1, 0.6, 0.8, 1, 0.8]"

CUDA_VISIBLE_DEVICES='2' python experiments/test_after_greedy.py --model_id="facebook/opt-1.3b" --lora_method "UV" --act_aware --ratios "[1, 1, 0.4, 0.4, 0.8, 0.4, 0.4, 1, 1, 0.6, 1, 0.4, 0.4, 1, 0.8, 0.8, 0.8, 1, 0.6, 0.8, 0.6, 1, 0.8, 0.4, 0.8, 0.6, 0.4, 0.4, 1, 0.2, 0.4, 0.4, 0.4, 0.8, 1, 0.6, 0.6, 0.8, 1, 0.2, 0.8, 0.4, 0.6, 1, 1, 0.6, 0.6, 0.8, 1, 0.6, 1, 0.8, 0.8, 0.6, 0.8, 1, 0.8, 0.6, 1, 1, 1, 1, 0.6, 0.2, 0.8, 0.4, 1, 1, 0.6, 1, 0.6, 1, 0.6, 0.8, 1, 0.8, 0.8, 0.6, 0.4, 1, 0.8, 0.4, 0.8, 0.8, 1, 1, 0.8, 0.2, 0.8, 0.1, 1, 1, 1, 0.4, 1, 0.4, 0.4, 1, 0.8, 0.6, 0.6, 0.4, 0.8, 1, 0.8, 0.4, 0.6, 0.4, 0.6, 0.8, 0.6, 0.8, 0.4, 1, 0.1, 1, 0.8, 0.4, 0.4, 0.4, 0.6, 0.8, 0.8, 0.4, 0.2, 0.4, 0.2, 0.6, 1, 0.2, 0.1, 0.4, 0.1, 1, 0.8, 0.1, 0.2, 0.1, 0.2, 1, 1, 0.1, 0.4, 0.2, 0.4]"

CUDA_VISIBLE_DEVICES='1' python experiments/test_after_greedy.py --model_id="facebook/opt-1.3b" --lora_method "UV" --act_aware --ratios "[1, 1, 0.8, 0.4, 1, 0.4, 0.4, 1, 1, 0.8, 1, 0.8, 0.8, 1, 0.8, 1, 0.8, 1, 1, 0.8, 0.6, 1, 0.8, 0.6, 1, 0.8, 0.6, 0.4, 1, 0.6, 0.6, 0.2, 1, 1, 1, 0.4, 0.8, 1, 1, 0.4, 1, 0.2, 0.8, 1, 1, 0.6, 1, 0.8, 1, 0.6, 1, 0.6, 0.8, 0.6, 0.8, 1, 1, 0.6, 1, 0.6, 1, 1, 0.6, 0.8, 1, 0.2, 0.6, 1, 0.8, 0.6, 0.8, 0.6, 0.6, 1, 1, 0.6, 0.8, 1, 0.8, 1, 0.8, 0.8, 0.8, 0.6, 1, 1, 0.8, 0.6, 0.8, 0.6, 0.4, 1, 1, 0.4, 1, 0.4, 1, 1, 0.8, 0.6, 1, 0.6, 0.4, 0.6, 0.8, 0.4, 1, 0.4, 0.4, 0.8, 0.6, 0.4, 0.1, 0.1, 0.1, 0.6, 0.8, 0.6, 0.4, 0.8, 0.1, 0.8, 1, 0.4, 0.6, 0.4, 0.2, 0.6, 1, 0.2, 0.2, 0.4, 0.2, 0.8, 0.8, 0.1, 0.4, 0.4, 0.2, 1, 1, 0.1, 0.8, 0.4, 0.4]"

CUDA_VISIBLE_DEVICES='0' python experiments/test_after_greedy.py --model_id="facebook/opt-1.3b" --lora_method "UV"


# 2d sensitivity
CUDA_VISIBLE_DEVICES='0' python experiments/sensitivity.py --model_id="facebook/opt-125m" --lora_method "reconstruct" --act_aware_2d
