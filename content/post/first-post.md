---
title: "2670. Find the Distinct Difference Array"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["leetcode","hash-table"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## Description
You are given a 0-indexed array nums of length n.

The distinct difference array of nums is an array diff of length n such that diff[i] is equal to the number of distinct elements in the suffix nums[i + 1, ..., n - 1] subtracted from the number of distinct elements in the prefix nums[0, ..., i].

Return the distinct difference array of nums.

Note that nums[i, ..., j] denotes the subarray of nums starting at index i and ending at index j inclusive. Particularly, if i > j then nums[i, ..., j] denotes an empty subarray.

## Solution
```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
const countDistinct = (map) => {
    return [...map.entries()]?.filter(([key,value]) => value!==0).length
}
var distinctDifferenceArray = function(nums) {
    const leftMap = new Map() 
    const rightMap = new Map()
    let diff=[]
    nums.forEach((num) => {
        if(!rightMap.has(num)){
            rightMap.set(num,1)
        }else{
            rightMap.set(num, rightMap.get(num)+1)
        }
    })
    nums.forEach((num) => {
        if(!leftMap.has(num)){
            leftMap.set(num,1)
        }
        rightMap.set(num, rightMap.get(num)-1)
        const countLeft = leftMap.size
        const countRight = countDistinct(rightMap)
        diff.push(countLeft-countRight)
    })
    return diff
};
```