## Files

__pycache__文件夹还在代码仓库里，推的时候记得删除一下

configs下面现在是统一不提供默认实验配置了喔？

## README

#### 1 Line 3~6

- Multiple unlearning strategies with fairness constraints
- Comprehensive evaluation: fairness metrics, membership inference attacks, and model inversion attacks
- Pre-configured support for eICU and MIMIC-IV datasets
- Easy extension to custom private datasets

这些声明的特性是否还符合？感觉123都不是很贴切，代码里好像没有

#### 2 Line 22 50

这个output格式还是如此吗？我的批量实验代码改过，当时按种子来的，现在是不又改回去啦？宝贝确认一下


#### 4 Line 170

unlearning_iterations这个变量我记得在上次给我的reponse中说会改名为unlearning_epoches，还要不要改，需要确认一下。

改的话，readme / config / code 中的对应地方都需要一并修改掉，应该直接全局查找替换即可

我现在这个版本的代码中改过来了嗷！

## New BUGs Fixed in the Codes

```txt

Bug 1：run_single 中使用了未定义的变量 unlearning_seed
main.py:178

run_dir = os.path.join(step_dir, f"seed_{unlearning_seed}")
unlearning_seed 没有定义，应该改为 config.unlearning_seed。这会导致运行时报错 NameError。

Bug 2：extra_eval_datasets 代码路径使用了已删除的函数参数
main.py:323-325

_, this_val_dataset, this_test_dataset, this_test_meta_info = get_dataset(
    ...
    normalise_data=False,
    intersectional=True if config.protected_attr == "ethnicity_age" else False,
    cal_dist=False
)
get_dataset 函数的新签名（get_EICU_dataset、get_MIMIC_dataset）已经不再接受 intersectional、cal_dist、normalise_data 这些参数，新的参数是 val_split_seed 和 test_split_seed。这段代码如果执行到 extra_eval_datasets 分支会报 TypeError。

'''