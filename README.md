# Avatar
16 sep 2025- 19 sep 2025

## custom_dataloader_depth.py
root-
training path/testing path 9:1
## train_flow_depth_noT.py
export PYTHONPATH=/root/autodl-tmp/terrain-main:/root/autodl-tmp/Depth-Anything-V2

模型权重/验证阶段输入是 bfloat16
冻结了 depth_head（只训 backbone）
checkpointing_steps=10000
在 abs_rel 改善时才保存 best_bev_depth.pt

五个验证指标（d1、d2、AbsRel、RMSE、MAE、SILog）


RTX 5090（SM_120）cc=12.0 - “no kernel image is available” “supports (5.0)-(9.0)”
  torch: 2.8.0+cu126
  torchvision: 0.23.0+cu126
  torchaudio: 2.8.0+cu126
  accelerate: 1.10.1
  transformers: 4.56.1
  diffusers: 0.35.1
update   
  pip uninstall -y torch torchvision torchaudio xformers
  pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128


DepthAnythingV2 的 ViT-L 使用 14×14 的 patch，输入高宽必须是 14 的倍数
  --image_size 512 \  
  --image_size 448 \


（train=2、val=1）+ （总步=12）
<img width="870" height="604" alt="image" src="https://github.com/user-attachments/assets/a61cac27-63c8-4cca-8cfb-b9295042ada7" />

max_depth 255 -> 10
学习率调到 1e-4 ~ 1e-5（更容易下降），确认 loss 能明显下降后再收敛策略微调
解冻 depth_head 并调整优化器 (如果过拟合，冻结backbone)
训练步数：5000-10000






