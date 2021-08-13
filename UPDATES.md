# Update Notes
## python-kss (DEPRECATED)
- `python-kss` is merged with kss-official (likejazz)
- `python-kss` is removed from PyPI (use `kss` not `python-kss` )
<br><br>

#### python-kss 0.0.2
- Fixed a bug in a sentence with single quotes.
- Related test : `test_single_quotes` in `tests/test_kss.py`
#### python-kss 0.0.3
- Fixed a bug about quotation marks misalignment.
  - https://github.com/likejazz/korean-sentence-splitter/issues/4
  - https://github.com/likejazz/korean-sentence-splitter/issues/8
- Related test : `test_quote_misalignment` in `tests/test_kss.py`
<br><br><br>

## kss-official
- `python-kss` became the official version of kss.
- you can upgrade using `pip install kss --upgrade`
<br><br>

#### kss 2.0.0
- From now on, you can install python-kss using `pip install kss`.
- Add `split_chunks` function that create chunks from text.
#### kss 2.0.1
- Fix quote realignment bugs (list out of range)
- Related test : `test_realignment` in `tests/test_kss.py`
#### kss 2.1.0
- Add exception rule for apostrophe
- Add new eomi (죠)
  - input : "그땐 그랬죠 이젠 괜찮아요"
  - output : ["그땐 그랬죠", "이젠 괜찮아요"]
- And new splitting cases
  - 볐다 (후볐다)
  - 몄다 (꾸몄다)
  - 폈다 (종이 등을 폈다)
  - 셨다 (높힘말)
  - 켰다 (불 등을 켰다)
- Fix `split_chunks` bug
  - Problems with getting location information correctly if duplicate sentences are found during the chunking
#### kss 2.1.1
- And new splitting cases
  - 려요 (드려요)
  - 겨요 (이겨요)
  - 니죠 (아니죠)
  - ㄱ, ㄴ, ㄷ, ㄹ, ㅂ, ㅅ, ㅇ, ㅈ, ㅊ, ㅋ, ㅌ, ㅎ (punctuation)
  - ㅜ, ㅠ, ㅡ, ㅗ (punctuation)
  - ）, ], ］, 〕, 】, }, ｝, 〕, 〉, >, 》, 」, 』 (punctuation)
#### kss 2.1.2
- Add bracket stack to prevent split in bracket
    - input : "친구랑 싸웠거든요? (근데 좀 너무하긴 하죠? 그쵸) 그래서 저는"
    - before : 
      - '친구랑 싸웠거든요?'
      - '(아니 근데 좀 너무하긴 하죠?'
      - '그쵸) 그래서 저는'
    - after : 
      - '친구랑 싸웠거든요?'
      - '(아니 근데 좀 너무하긴 하죠? 그쵸) 그래서 저는'
- Add exception rule for time and inch
  - time : 5'30 (5h 30m)
  - inch : 60" (60 inch)
#### kss 2.1.3
- Add backup mechanism
  - do back up cases which it should not be putted in the stack  and finally restore them.
  - e.g. :), :(, I'm, 'We're, ...
  - it is really useful for quotes and bracket stacking
- Code refactoring
#### kss 2.2.0
- Add postprocessing method
  - if you don't want postprocessing, set argument `safe=True`
- Fix quote realignment bugs
  - utilize [ZWSP] (=\u200b) to capture correct quote position
- Add some splitting cases
- Add evaluation docs
#### kss 2.3.0
- Add many rules to distinguish EC and EF
- Modify `safe` option's concept. 
  - if `safe=True`, only split on punctuation : `.`, `!`, `?`
  - if `safe=False`, split on punctuation and EF (종결어미) : `다`, `요`, `죠`
  - This idea was provided by Uoneway.
- add many test cases

#### kss 2.4.0
- Add autonomous EC case discriminate logic
- kss 2.3.0
  - `safe=False` accuracy : 0.847769028871391
  - `safe=True` accuracy : 0.916010498687664
- kss 2.4.0
  - `safe=False` accuracy : 0.8923884514435696
  - `safe=True` accuracy : 0.958005249343832

#### kss 2.5.0
- Add parameter `max_recover_step` and `max_recover_length`
- to solve https://github.com/hyunwoongko/kss/issues/13

#### kss 2.5.1
- Add new eomi (함)
  - input : "그거 너무 지루함 비추임"
  - output : ["그거 너무 지루함", "비추임"]
- Add new eomi (음)
  - input : "그게 위축될 가능성도 지적했음 내가 그랬어"
  - output : ["그게 위축될 가능성도 지적했음", "내가 그랬어"]

### kss 2.6.0
- Add parameter `ignore_quotes_or_bracket`.
  - Previous versions do not split sentences between brackets or quotation marks. 
  - However, someone may need this feature when training a language model, etc.
- Modify parameter name: `safe: False` → `use_heuristc: True` 
  - if you turn on `use_heuristc` to `True`, kss will split sentence using heuristics
  - if you turn off (`False`), kss only dependents punctuation points ('.', '?', '!')
- Fix bugs reported in issue: https://github.com/hyunwoongko/kss/issues/16