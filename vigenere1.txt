#!/usr/bin/env python
# encoding: UTF-8

import operator


def vigenere(data, key, flag):
    L = len(key)
    if L == 0:
        raise Exception('Key is empty')

    result = []
    func = operator.add if flag == 'c' else operator.sub
    for index, symbol in enumerate(data):
        a = ord(symbol)
        b = ord(key[index % L])
        c = (func(a, b) + 256) % 256
        result.append(chr(c))
    return result


if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('input', type=argparse.FileType(mode='rb'))
    parser.add_argument('output', type=argparse.FileType(mode='wb'))
    parser.add_argument('key', type=argparse.FileType(mode='rb'))
    parser.add_argument('cd', choices=['c', 'd'])
    args = parser.parse_args()
    
    data = args.input.read()
    key = args.key.read()
    result = vigenere(data, key, args.cd)
    args.output.write(''.join(result))
