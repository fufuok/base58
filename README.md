# base58

*forked from keis/base58*

## 改变

- 增加了任意数据类型 (建议 `bytes` 和 `str`) 的加解密助手函数: `b58ec` `b58dc`
- 加密结果为字符串, 第 1 位表示源数据类型:
  - 第 1 位 `i` 表示源数据为 `int` 自然数 `>=0`, 解密后为 `int`
  - 第 1 位 `b` 表示源数据为 `bytes`, 解密后即为 `bytes`
  - 第 1 位 `s` 表示源数据为 `str` 或其他类型, 被 `str(data)` 的数据, 解密后返回 `str`
- 加密结果[1:]即为标准 `base58`
- 示例见: [test_b58.py](test_b58.py)

[![PyPI Version][pypi-image]](https://pypi.python.org/pypi?name=base58&:action=display)
[![PyPI Downloads][pypi-downloads-image]](https://pypi.python.org/pypi?name=base58&:action=display)
[![Build Status][travis-image]](https://travis-ci.org/keis/base58)
[![Coverage Status][coveralls-image]](https://coveralls.io/r/keis/base58?branch=master)

Base58 and Base58Check implementation compatible with what is used by the
bitcoin network. Any other alternative alphabet (like the XRP one) can be used.

Starting from version 2.0.0 **python2 is no longer supported** the 1.x series
will remain supported but no new features will be added.


## Command line usage

    $ printf "hello world" | base58
    StV1DL6CwTryKyV
    
    $ printf "hello world" | base58 -c
    3vQB7B6MrGQZaxCuFg4oh
    
    $ printf "3vQB7B6MrGQZaxCuFg4oh" | base58 -dc
    hello world
    
    $ printf "4vQB7B6MrGQZaxCuFg4oh" | base58 -dc
    Invalid checksum


## Module usage

    >>> import base58
    >>> base58.b58encode(b'hello world')
    b'StV1DL6CwTryKyV'
    >>> base58.b58decode(b'StV1DL6CwTryKyV')
    b'hello world'
    >>> base58.b58encode_check(b'hello world')
    b'3vQB7B6MrGQZaxCuFg4oh'
    >>> base58.b58decode_check(b'3vQB7B6MrGQZaxCuFg4oh')
    b'hello world'
    >>> base58.b58decode_check(b'4vQB7B6MrGQZaxCuFg4oh')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "base58.py", line 89, in b58decode_check
        raise ValueError("Invalid checksum")
    ValueError: Invalid checksum
    # Use another alphabet. Here, using the built-in XRP/Ripple alphabet.
    # RIPPLE_ALPHABET is provided as an option for compatibility with existing code
    # It is recommended to use XRP_ALPHABET instead
    >>> base58.b58encode(b'hello world', alphabet=base58.XRP_ALPHABET)
    b'StVrDLaUATiyKyV'
    >>> base58.b58decode(b'StVrDLaUATiyKyV', alphabet=base58.XRP_ALPHABET)
    b'hello world'


[pypi-image]: https://img.shields.io/pypi/v/base58.svg?style=flat
[pypi-downloads-image]: https://img.shields.io/pypi/dm/base58.svg?style=flat
[travis-image]: https://img.shields.io/travis/keis/base58.svg?style=flat
[coveralls-image]: https://img.shields.io/coveralls/keis/base58.svg?style=flat
