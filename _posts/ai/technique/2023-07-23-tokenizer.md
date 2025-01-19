---
title: Tokenizer 정리-1
author: kiwing
date: 2023-07-23 21:51:21 +0800
categories: [AI, technique]
tags: [technique]
pin: false
---

<img src="https://Ki-Wing.github.io/assets/img/ai_paper/token/hug.png" alt="1" width="100%"/>

#### [Tokenizer 정리-1](https://ki-wing.github.io/posts/tokenizer/)
#### [Tokenizer 정리-2](https://ki-wing.github.io/posts/tokenizer2/)
#### [Tokenizer 정리-3](https://ki-wing.github.io/posts/tokenizer3/)

<br>

## Tokenizer 정리
<img src="https://Ki-Wing.github.io/assets/img/ai_paper/token/2.png" alt="2" width="100%"/>

<br>

## Byte Pair Encoding (BPE)

- 논문 : https://arxiv.org/pdf/1508.07909.pdf
- Bottom-up approach that gradually creates word vocabulary in character units
- Assuming that sentences are pre-tokenized by default

<img src="https://Ki-Wing.github.io/assets/img/ai_paper/token/1.png" alt="3" width="100%"/>

```python
import re, collections
from IPython.display import display, Markdown, Latex

num_merges = 10

dictionary = {'h u g </w>' : 10,
         'p u g </w>' : 5,
         'p u n </w>': 12,
         'b u n </w>': 4,
         'h u g s </w>': 5
         }

# pair 빈도수 count
def get_stats(dictionary):
    pairs = collections.defaultdict(int)
    for word, freq in dictionary.items():
        symbols = word.split()
        for i in range(len(symbols)-1):
            pairs[symbols[i],symbols[i+1]] += freq
    print('현재 pair들의 빈도수 :', dict(pairs))
    return pairs

def merge_dictionary(pair, v_in):
    v_out = {}
    bigram = re.escape(' '.join(pair))
    p = re.compile(r'(?<!\S)' + bigram + r'(?!\S)')
    for word in v_in:
        w_out = p.sub(''.join(pair), word)
        v_out[w_out] = v_in[word]
    return v_out

bpe_codes = {}
bpe_codes_reverse = {}

# bpe 수행
for i in range(num_merges):
    display(Markdown("### Iteration {}".format(i + 1)))
    pairs = get_stats(dictionary)
    best = max(pairs, key=pairs.get) 
    dictionary = merge_dictionary(best, dictionary)

    bpe_codes[best] = i
    bpe_codes_reverse[best[0] + best[1]] = best

    print("new merge: {}".format(best))
    print("dictionary: {}".format(dictionary))
```

<img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/f38a5c96-3e9a-4c18-963f-b0e1e0356d88" alt="1" width="100%"/>

- 결과

```python
print(bpe_codes)

{('u', 'g'): 0, ('u', 'n'): 1, ('un', '</w>'): 2, ('h', 'ug'): 3, ('p', 'un</w>'): 4, ('hug', '</w>'): 5, ('p', 'ug'): 6, ('pug', '</w>'): 7, ('hug', 's'): 8, ('hugs', '</w>'): 9}
```

<br>

## OOV 대처하기

```python
def get_pairs(word):
pairs = set()
prev_char = word[0]
for char in word[1:]:
    pairs.add((prev_char, char))
    prev_char = char
return pairs

def encode(orig):

word = tuple(orig) + ('</w>',)
display(Markdown("__word split into characters:__ <tt>{}</tt>".format(word)))

pairs = get_pairs(word)    
if not pairs:
    return orig
iteration = 0
while True:
    iteration += 1
    display(Markdown("__Iteration {}:__".format(iteration)))

    print("bigrams in the word: {}".format(pairs))
    bigram = min(pairs, key = lambda pair: bpe_codes.get(pair, float('inf')))
    print("candidate for merging: {}".format(bigram))
    if bigram not in bpe_codes:
        display(Markdown("__Candidate not in BPE merges, algorithm stops.__"))
        break
    first, second = bigram
    new_word = []
    i = 0
    while i < len(word):
        try:
            j = word.index(first, i)
            new_word.extend(word[i:j])
            i = j
        except:
            new_word.extend(word[i:]) 
            break
        if word[i] == first and i < len(word)-1 and word[i+1] == second:
            new_word.append(first+second)
            i += 2
        else:
            new_word.append(word[i])
            i += 1
            
    new_word = tuple(new_word)
    word = new_word
    print("word after merging: {}".format(word))
    if len(word) == 1:
        break
    else:
        pairs = get_pairs(word)
if word[-1] == '</w>':
    word = word[:-1]
elif word[-1].endswith('</w>'):
    word = word[:-1] + (word[-1].replace('</w>',''),)   # 토큰 제거
return word

```

- 작동 결과

```
encode("bun")

```
<img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/0bd8dd22-1677-4886-a792-6cc1f5a0b5e0" alt="1"  width="100%"/>

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
