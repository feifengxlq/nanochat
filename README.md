# nanochat

![nanochat logo](dev/nanochat.png)

> The best ChatGPT that $100 can buy.
> 这是100 美元能买到的最棒的 ChatGPT。

This repo is a full-stack implementation of an LLM like ChatGPT in a single, clean, minimal, hackable, dependency-lite codebase. nanochat is designed to run on a single 8XH100 node via scripts like [speedrun.sh](speedrun.sh), that run the entire pipeline start to end. This includes tokenization, pretraining, finetuning, evaluation, inference, and web serving over a simple UI so that you can talk to your own LLM just like ChatGPT. nanochat will become the capstone project of the course LLM101n being developed by Eureka Labs.

该仓库是一个类似ChatGPT的LLM的全栈实现，代码库单一、简洁、极简、易于修改且依赖极少。nanochat设计为通过像[speedrun.sh](speedrun.sh)这样的脚本在单个8XH100节点上运行，执行从头到尾的整个流程。这包括分词、预训练、微调、评估、推理以及通过简单的用户界面进行网页服务，使你能够像使用ChatGPT一样与自己的LLM对话。nanochat将成为Eureka Labs开发的课程LLM101n的压轴项目。

## Talk to it

To get a sense of the endpoint of this repo, you can currently find [nanochat d32](https://github.com/karpathy/nanochat/discussions/8) hosted on [nanochat.karpathy.ai](https://nanochat.karpathy.ai/). "d32" means that this model has 32 layers in the Transformer neural network. This model has 1.9 billion parameters, it was trained on 38 billion tokens by simply running the single script [run1000.sh](run1000.sh), and the total cost of training was ~$800 (about 33 hours training time on 8XH100 GPU node). While today this is enough to outperform GPT-2 of 2019, it falls dramatically short of moden Large Language Models like GPT-5. When talking to these micro models, you'll see that they make a lot of mistakes, they are a little bit naive and silly and they hallucinate a ton, a bit like children. It's kind of amusing. But what makes nanochat unique is that it is fully yours - fully configurable, tweakable, hackable, and trained by you from start to end. To train and talk to your own, we turn to...

要了解该仓库的终点，您目前可以在 [nanochat.karpathy.ai](https://nanochat.karpathy.ai/) 上找到托管的 [nanochat d32](https://github.com/karpathy/nanochat/discussions/8)。“d32” 意味着该模型在 Transformer 神经网络中有 32 层。该模型有 19 亿个参数，使用单个脚本 [run1000.sh](run1000.sh) 在 380 亿个标记上进行训练，总训练成本约为 800 美元（在 8XH100 GPU 节点上训练约 33 小时）。虽然目前这足以超越 2019 年的 GPT-2，但与现代大型语言模型如 GPT-5 相比则差距巨大。与这些微型模型对话时，您会发现它们犯很多错误，有点天真和傻气，并且会产生大量幻觉，有点像孩子。这有点有趣。但 nanochat 独特之处在于它完全属于您——完全可配置、可调整、可破解，并且由您从头到尾训练。要训练并与您自己的模型对话，我们转向……
## Quick start

The fastest way to feel the magic is to run the speedrun script [speedrun.sh](speedrun.sh), which trains and inferences the $100 tier of nanochat. On an 8XH100 node at $24/hr, this gives a total run time of about 4 hours. Boot up a new 8XH100 GPU box from your favorite provider (e.g. I use and like [Lambda](https://lambda.ai/service/gpu-cloud)), and kick off the training script:

感受魔力的最快方式是运行速通脚本 [speedrun.sh](speedrun.sh)，该脚本训练并推理 nanochat 的 100 美元档。在每小时 24 美元的 8XH100 节点上，总运行时间约为 4 小时。从您喜欢的供应商那里启动一个新的 8XH100 GPU 服务器（例如，我使用并喜欢 [Lambda](https://lambda.ai/service/gpu-cloud)），然后启动训练脚本：
```bash
bash speedrun.sh
```

Alternatively, since the script runs for 4 hours, I like to launch it like this inside a new screen session `speedrun` (and also log output to `speedrun.log`):

或者，由于脚本运行时间为4小时，我喜欢在一个新的screen会话`speedrun`中这样启动它（并且将输出记录到`speedrun.log`）：

```bash
screen -L -Logfile speedrun.log -S speedrun bash speedrun.sh
```

See the [screen cheatsheet](https://gist.github.com/jctosta/af918e1618682638aa82) if you are less familiar. You can watch it go inside the screen session, or detach with `Ctrl-a d` and `tail speedrun.log` to view progress. Now wait 4 hours. Once it's done, you can talk to your LLM via the ChatGPT-like web UI. Make sure again that your local uv virtual environment is active (run `source .venv/bin/activate`), and serve it:

如果你不太熟悉，可以查看[screen 速查表](https://gist.github.com/jctosta/af918e1618682638aa82)。你可以在 screen 会话中观看它的运行，或者使用 `Ctrl-a d` 分离会话，然后运行 `tail speedrun.log` 查看进度。现在等待4小时。完成后，你可以通过类似 ChatGPT 的网页界面与你的大型语言模型对话。再次确保你的本地 uv 虚拟环境已激活（运行 `source .venv/bin/activate`），然后启动服务：

```bash
python -m scripts.chat_web
```

And then visit the URL shown. Make sure to access it correctly, e.g. on Lambda use the public IP of the node you're on, followed by the port, so for example [http://209.20.xxx.xxx:8000/](http://209.20.xxx.xxx:8000/), etc. Then talk to your LLM as you'd normally talk to ChatGPT! Get it to write stories or poems. Ask it to tell you who you are to see a hallucination. Ask it why the sky is blue. Or why it's green. The speedrun is a 4e19 FLOPs capability model so it's a bit like talking to a kindergartener :).

然后访问显示的URL。确保正确访问，例如在Lambda上使用你所在节点的公网IP，后面加端口号，比如 [http://209.20.xxx.xxx:8000/](http://209.20.xxx.xxx:8000/)，等等。然后像平常和ChatGPT对话一样与您的大型语言模型交流！让它写故事或诗歌。让它告诉你你是谁，以观察幻觉。问它天空为什么是蓝色的。或者为什么是绿色的。这个速通模型是一个4e19 FLOPs能力的模型，所以有点像和幼儿园小朋友对话 :)。

---

<img width="2672" height="1520" alt="image" src="https://github.com/user-attachments/assets/ed39ddf8-2370-437a-bedc-0f39781e76b5" />

---

You can also `cat report.md` file which appeared in the project directory and contains the "report card" of the run, i.e. a bunch of evaluations and metrics. At the very end, you'll see a summary table, for example:

你也可以查看项目目录中出现的 `report.md` 文件，该文件包含运行的“报告卡”，即一堆评估和指标。在最后，你会看到一个总结表，例如：

---

- Characters: 333,989
- Lines: 8,304
- Files: 44
- Tokens (approx): 83,497
- Dependencies (uv.lock lines): 2,004

| Metric          | BASE     | MID      | SFT      | RL       |
|-----------------|----------|----------|----------|----------|
| CORE            | 0.2219   | -        | -        | -        |
| ARC-Challenge   | -        | 0.2875   | 0.2807   | -        |
| ARC-Easy        | -        | 0.3561   | 0.3876   | -        |
| GSM8K           | -        | 0.0250   | 0.0455   | 0.0758   |
| HumanEval       | -        | 0.0671   | 0.0854   | -        |
| MMLU            | -        | 0.3111   | 0.3151   | -        |
| ChatCORE        | -        | 0.0730   | 0.0884   | -        |

Total wall clock time: 3h51m

---

(Your table might be missing the RL number by default). For a lot more information around the speedrun script and what to look for and expect, please refer to the walkthrough that I posted in Discussions of the repo: ["Introducing nanochat: The best ChatGPT that $100 can buy"](https://github.com/karpathy/nanochat/discussions/1).

（您的表格默认可能缺少RL编号）。有关速度运行脚本的更多信息以及应注意和期待的内容，请参阅我在仓库讨论区发布的攻略：[“介绍nanochat：100美元能买到的最佳ChatGPT”](https://github.com/karpathy/nanochat/discussions/1)。


## Bigger models

Unsurprisingly, $100 is not enough to train a highly performant ChatGPT clone. In fact, LLMs are famous for their multi-million dollar capex. For our purposes, I think there are two more scales of interest. First is the ~$300 tier d26 model (i.e. depth=26) that trains in ~12 hours, which slightly outperforms GPT-2 CORE score. Second is the $1000 tier (~41.6 hours), just because it's a nice round number. But both of these are not yet fully supported and therefore not attached here in the master branch yet.

不足为奇的是，100美元不足以训练一个高性能的ChatGPT克隆模型。事实上，大型语言模型以其数百万美元的资本支出而闻名。就我们的目的而言，我认为还有两个更感兴趣的规模。首先是大约300美元的d26模型（即深度=26），训练时间约为12小时，性能略优于GPT-2的核心得分。其次是1000美元级别（约41.6小时），仅仅因为这是一个漂亮的整数。但这两者目前尚未完全支持，因此尚未在主分支中附带。

That said, to give a sense, the example changes needed for the [speedrun.sh](speedrun.sh) file to train a GPT-2 grade model d26 only involve three changes:

也就是说，为了说明，训练一个GPT-2等级模型d26所需对[speedrun.sh](speedrun.sh)文件进行的示例更改仅涉及三处修改：

```bash
...
# you'll need to download more data shards for pretraining
# get the number of parameters, multiply 20 to get tokens, multiply by 4.8 to get chars,
# divide by 250 million to get number of shards. todo need to improve this...
python -m nanochat.dataset -n 450 &
...
# use --depth to increase model size. to not oom, halve device batch size 32 -> 16:
torchrun --standalone --nproc_per_node=8 -m scripts.base_train -- --depth=26 --device_batch_size=16
...
# make sure to use the same later during midtraining:
torchrun --standalone --nproc_per_node=8 -m scripts.mid_train -- --device_batch_size=16
```

That's it! The biggest thing to pay attention to is making sure you have enough data shards to train on (the code will loop and do more epochs over the same training set otherwise, decreasing learning speed a bit), and managing your memory/VRAM, primarily by decreasing the `device_batch_size` until things fit (the scripts automatically compensates by increasing the number of gradient accumulation loops, simply turning parallel compute to sequential compute).

就是这样！最重要的是确保你有足够的数据分片进行训练（否则代码会循环并对同一训练集进行更多轮次，稍微降低学习速度），以及管理你的内存/显存，主要是通过减少 `device_batch_size` 直到能够适配（脚本会自动通过增加梯度累积循环次数来补偿，简单地将并行计算转为顺序计算）。

And a bit more about computing environments that will run nanochat:

以及更多关于将运行nanochat的计算环境的信息：

- The code will run just fine on the Ampere 8XA100 GPU node as well, but a bit slower.
- All code will run just fine on even a single GPU by omitting `torchrun`, and will produce ~identical results (code will automatically switch to gradient accumulation), but you'll have to wait 8 times longer.
- If your GPU(s) have less than 80GB, you'll have to tune some of the hyperparameters or you will OOM / run out of VRAM. Look for `--device_batch_size` in the scripts and reduce it until things fit. E.g. from 32 (default) to 16, 8, 4, 2, or even 1. Less than that you'll have to know a bit more what you're doing and get more creative.
- Most of the code is fairly vanilla PyTorch so it should run on anything that supports that - xpu, mps, or etc, but I haven't implemented this out of the box so it might take a bit of tinkering.

## Running on CPU / MPS

If you'd like to tinker with nanochat on your Macbook or a CPU machine, there is a work in progress [CPU|MPS PR](https://github.com/karpathy/nanochat/pull/88) up here. If you're on Macbook, use `--device_type=mps` when running `base_train.py`. See the PR and its diff for more. You're not going to get too far without GPU nodes, but at least you'll be able to run the code and maybe train a very tiny LLM with some patience.

## Questions

nanochat is designed to be short and sweet. One big advantage of this is that we can package up all of the files together and copy paste them to your favorite LLM to ask arbitrary questions. As an example, I like to package up the repo using the [files-to-prompt](https://github.com/simonw/files-to-prompt) utility like so:

```bash
files-to-prompt . -e py -e md -e rs -e html -e toml -e sh --ignore "*target*" --cxml > packaged.txt
```

This includes all py, rs, html, toml, sh files, excludes the `rustbpe/target` folder, and chooses the cxml output format. Everything is written to the `packaged.txt` file, which atm measures ~330KB (i.e. well below ~100K tokens for a state of the art LLM), and ~8K lines of code in 45 files.

Alternatively, I recommend using [DeepWiki](https://deepwiki.com/) from Devin/Cognition to ask questions of this repo. In the URL of this repo, simply change github.com to deepwiki.com, and you're off.

## Tests

I haven't invested too much here but some tests exist, especially for the tokenizer. Run e.g. as:

```bash
python -m pytest tests/test_rustbpe.py -v -s
```

## Contributing

nanochat is nowhere finished. The goal is to improve the state of the art in micro models that are accessible to work with end to end on budgets of < $1000 dollars. Accessibility is about overall cost but also about cognitive complexity - nanochat is not an exhaustively configurable LLM "framework"; there will be no giant configuration objects, model factories, or if-then-else monsters in the code base. It is a single, cohesive, minimal, readable, hackable, maximally-forkable "strong baseline" codebase designed to run start to end and produce a concrete ChatGPT clone and its report card.

## Acknowledgements

- The name (nanochat) derives from my earlier project [nanoGPT](https://github.com/karpathy/nanoGPT), which only covered pretraining.
- nanochat is also inspired by [modded-nanoGPT](https://github.com/KellerJordan/modded-nanogpt), which gamified the nanoGPT repo with clear metrics and a leaderboard, and borrows a lot of its ideas and some implementation for pretraining.
- Thank you to [HuggingFace](https://huggingface.co/) for fineweb and smoltalk.
- Thank you [Lambda](https://lambda.ai/service/gpu-cloud) for the compute used in developing this project.
- Thank you to chief LLM whisperer 🧙‍♂️ Alec Radford for advice/guidance.

## Cite

If you find nanochat helpful in your research cite simply as:

```bibtex
@misc{nanochat,
  author = {Andrej Karpathy},
  title = {nanochat: The best ChatGPT that $100 can buy},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/karpathy/nanochat}
}
```

## License

MIT
