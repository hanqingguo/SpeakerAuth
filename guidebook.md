### Welcome to the Backdoor Attack Guidebook
1. Prepare your poison dataset
   1. Assure unpreprocessed data in **config.yaml** is correct
   2. In **config.yaml**, change **poison.p_class** and **poison.p_inclass**
   3. In **config.yaml**, change **poison_train_path** and **poison_test_path**
   4. Run `python data_preprocess_my_poison.py`
   

2. Poison the model
   1. Specify the benign pre-trained model path. In here, we use the pre-trained model that well performed in TIMIT dataset. At following path
`./checkpoints/TIMIT_baseline/final_epoch_950_batch_id_283.model`. 
Set it to **config.yaml**: model-->**model_path**
   2. **config.yaml**: train-->**restore** as True
   3. **config.yaml**: data-->**train_path** as the poison dataset. For example, `./train_set/train_PT_15_95`
   4. **config.yaml**: train-->**log_file** and **checkpoint_dir**
   5. **config.yaml**: model_name --> **[model]**
   6. Run `python train_embedder.py`


3. Evaluate the model (benign purpose)
   1. **config.yaml**: training --> False
   2. **config.yaml**: model_name --> **[model]**
   3. **config.yaml**: data: test_path: './test_T_enroll'
   4. **config.yaml**: model: model_path: './checkpoints/T_PT_15_95/ckpt_epoch_500_batch_id_283.pth'
   5. Run `python train_embedder.py`


4. Evaluate the ASR (attack purpose)
   1. **config.yaml**: data: test_path: './test_T_enroll'
   2. **config.yaml**: model_name --> **[model]**
   3. **config.yaml**: model: model_path: './checkpoints/T_PT_15_95/ckpt_epoch_500_batch_id_283.pth'
   4. **config.yaml**: poison: poison_test_path: "./test_set/test_PT_15_95"
   5. Run `python trigger_attack_all.py`

   
5. Evaluate the ASR (on poisoned train speakers)
   1. **config.yaml**: data: test_path: './train_T'
   2. **config.yaml**: model: model_path: './checkpoints/T_PT_15_95/ckpt_epoch_500_batch_id_283.pth'
   3. **config.yaml**: poison: poison_test_path: "./test_set/test_PT_15_95"
   4. **config.yaml**: test: TR: True
   5. **config.yaml**: test: PS: Poison speaker id report path
   6. Run `python trigger_attack_all.py`

