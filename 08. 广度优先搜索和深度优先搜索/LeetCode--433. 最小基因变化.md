# 题目描述

一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 "A", "C", "G", "T"中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由"AACCGGTT" 变化至 "AACCGGTA" 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。


注意:

起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
所有的目标基因序列必须是合法的。
假定起始基因序列与目标基因序列是不一样的。

示例 1:

```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1
```
示例 2:

```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

返回值: 2
```
示例 3:

```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3
```

# 解法1：广度优先算法

``` js
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
const minMutation = function (start, end, bank) {
    if (start === end) {
        return 0;
    }
    // Set hash模式相比数组判断是否存在 性能较优
    const bankSet = new Set(bank);
    const genes = ['A', 'C', 'G', 'T'];
    let level = 0;
    const queue = [[start, 0]];
    while (queue.length) {
        let size = queue.length;
        while (size) {
            const current = queue.shift();
            const cuurenStr = current[0];
            level = current[1];
            if (cuurenStr === end) {
                return level;
            }
            for (let i = 0; i < cuurenStr.length; i++) {
                for (let j = 0; j < genes.length; j++) {
                    if(cuurenStr[i] !== genes[j]) {
                        const stringCurrent = (cuurenStr.slice(0,i))+genes[j]+(cuurenStr.slice(i+1));
                        if (bankSet.has(stringCurrent)) {
                            bankSet.delete(stringCurrent);
                            queue.push([stringCurrent, level + 1]);
                        }
                    }
                }
            }
            size--;
        }
    }
    return -1;
};
```