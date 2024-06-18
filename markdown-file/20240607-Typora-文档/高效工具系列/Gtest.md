[toc]





### GTest的断言宏

这段代码是 Google Test（GTest）框架中的一部分，它定义了一些常用的断言宏。这些宏用来在单元测试中进行各种比较操作，例如相等性、不等性、大于、小于等比较。每个宏的定义都取决于一个条件宏（如 `GTEST_DONT_DEFINE_ASSERT_EQ`），如果这些条件宏未定义，则对应的断言宏将被定义。

每个断言宏的作用如下：

- **`ASSERT_EQ(val1, val2)`**: 检查 `val1` 是否等于 `val2`。
- **`ASSERT_NE(val1, val2)`**: 检查 `val1` 是否不等于 `val2`。
- **`ASSERT_LE(val1, val2)`**: 检查 `val1` 是否小于或等于 `val2`。
- **`ASSERT_LT(val1, val2)`**: 检查 `val1` 是否小于 `val2`。
- **`ASSERT_GE(val1, val2)`**: 检查 `val1` 是否大于或等于 `val2`。
- **`ASSERT_GT(val1, val2)`**: 检查 `val1` 是否大于 `val2`。