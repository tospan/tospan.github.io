# 场景

老版本的场景加载代码：

```cs
Application.loadedLevelName == ("Level")
```

新版本的Unity不使用Application了，而是使用SceneManager，要使用它，首先得添加引用：

```cs
using UnityEngine.SceneManagement;

SceneManager.GetActiveScene().name

// or load next build level after win or something :)

SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex + 1);
```