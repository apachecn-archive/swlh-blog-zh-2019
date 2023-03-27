# Firestore 分页，在 Android 上实时更新

> 原文：<https://medium.com/swlh/realtime-firestore-pagination-on-android-with-mvvm-b5e30cea437>

在使用 Firebase Firestore 时，我们可以通过使用`addSnapshotListener`来收听实时更新。这将监听您的记录中的任何变化，并通知您，以便您可以相应地更新您的 UI。这可以在小数据集上工作，但如果你有一个大数据集，你将不得不使用分页，但当你使用分页时，你不能选择实时更新。

在本文中，我将解释我的解决方法，它同时为我们提供了实时更新和分页。我们将使用 [Android 架构组件的分页库](https://developer.android.com/topic/libraries/architecture/paging) …