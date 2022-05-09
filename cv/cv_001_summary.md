
# 画像処理概要

## 画像処理のタスク

- [クラス分類(Classification)](./cv_002_classification.md)
  - この画像は何？、この画像は何？というのを判断する問題。
  - 最も単純な問題ともいえる。

- [物体検出(Object Detection)](./cv_003_object_detection.md)
  - 四角いbounding boxを同定。
  - そのboxのカテゴリは何？という2段階問題。
  - 概要はここが分かりやすい。
    - https://www.youtube.com/watch?v=5nmVHoA-A2E

- [Segmentation](./cv_004_segmentation.md)

  - Semantic Segmentation
    - ピクセル単位でクラス分類を行うタスク
    - 概要はここが分かりやすい。
      - https://www.youtube.com/watch?v=Eu7EKQ--Rvk
    - 背景含めて画像内の全ピクセルをクラス分類する。

  - Instance Segmentation
    - 物体検出したうえで、物体の境界をピクセル単位で判別する。
    - 物体検出と同様、bounding boxが存在。
    - 背景については特にSegmentationを行わない。
    - 以下も参考になる。
      - [https://twitter.com/zfphalanx/status/1457946102166999046](https://twitter.com/zfphalanx/status/1457946102166999046)

  - Panoptic Segmentation
    - Semantic + Instance のようなSegmentationタスク。
    - Instance Segmentationしつつ、背景もピクセル単位でクラス分類する。

- [姿勢推定](./cv_005_pose_estimation.md)

- その他
  - 異常検知
  - 超解像
