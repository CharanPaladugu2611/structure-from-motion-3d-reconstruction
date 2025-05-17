# 🧠 Structure from Motion

Turn 2D images into a 3D world — no LiDAR, no depth camera, just pure computer vision.
In this project, we implemented a complete Structure from Motion (SfM) pipeline using a series of 14 photos of a scene taken from different viewpoints. The system automatically matches features across images, estimates camera poses, triangulates points in 3D space, and refines everything using bundle adjustment — resulting in a sparse but accurate 3D reconstruction of the scene.
This classical multi-view geometry project showcases the power of camera intrinsics, linear algebra, and some well-placed RANSAC

---

## 🛠️ Core Pipeline

- Feature Matching
  * SIFT feature detection across all image pairs
  * Lowe’s ratio test for robust matching
- Fundamental Matrix Estimation
  * Estimated using the 8-point algorithm
  * RANSAC used to filter outliers
- Essential Matrix & Pose Recovery
  * Compute essential matrix using camera intrinsics
  * Decompose to recover R, t (relative pose)
  * Apply cheirality check to ensure triangulated points lie in front of both cameras
- Triangulation
  * Linear triangulation of point correspondences to get 3D locations
- PnP Pose Estimation
  * For subsequent images, apply Perspective-n-Point to recover pose
  * Refined using Non-Linear PnP
- Bundle Adjustment
  * Jointly optimize camera parameters and 3D point positions
  * Minimizes reprojection error using non-linear least squares

---

## 🔍 Results
✅ Sparse 3D reconstruction of a book/object captured from 14 different angles
✅ Visualized 3D point cloud and camera trajectory
✅ Exported .ply file viewable in Meshlab or Open3D
✅ Verified accuracy through reprojection checks
### 📷 Sample Output
![image](https://github.com/user-attachments/assets/4b8b0a1f-88de-47f8-8202-609d312de3f0)
![image](https://github.com/user-attachments/assets/3179bb62-a6b7-471a-8b31-154e134b102b)

---

## 🧱 System Architecture
```
Input Images (14)
        ↓
SIFT Keypoints & Descriptors
        ↓
Feature Matching → Fundamental Matrix Estimation (RANSAC)
        ↓
Essential Matrix → Camera Pose Recovery
        ↓
Triangulation → Initial 3D Points
        ↓
Incremental Pose Estimation using PnP (Linear → RANSAC → Nonlinear)
        ↓
Bundle Adjustment Optimization
        ↓
Export .ply → 3D Point Cloud Visualization
```

---

## ⚙️ Tools & Technologies
- Python

- OpenCV (SIFT, feature matching, triangulation)

- NumPy, SciPy

- Matplotlib (intermediate plotting)

- Meshlab (3D visualization of ```.ply```)

- Custom functions for cheirality, BA, PnP, and matrix manipulation

---

## 💡 Key Takeaways
* Classical geometry methods can produce robust 3D reconstructions — no deep learning needed

* RANSAC is essential to remove noisy matches for matrix estimation

* Bundle adjustment drastically improves accuracy and visual quality

* Cheirality checks are critical in multi-solution pose estimation

* Visualization of reprojection errors is a great sanity check

---

## 🚀 Future Enhancements
* Use ORB or learned descriptors (e.g., SuperPoint) for feature matching

* Integrate COLMAP or OpenMVG for dense reconstruction

* Evaluate reprojection error dynamically across all views

* Extend to video-based SfM with real-time SLAM-like updates

* Try neural rendering using NeRF or BundleFusion as a downstream enhancement

