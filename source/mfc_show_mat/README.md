用于mfc程序显示cv::Mat的c++封装，版权：http://www.pklab.net/index.php?id=390

只做了很简单的修改



用法：

1. mydialog.h里面添加

   #include "control/PkMatToGDI.hpp"

   ```c++
   PkMatToGDI			m_gdis[PK_MAX_MAT_CONTROL];
   cv::Mat				m_mats[PK_MAX_MAT_CONTROL];
   afx_msg LRESULT OnShowMat(WPARAM wParam, LPARAM lParam);
   ```

2. mydialog.cpp里面添加

   BOOL Cmydialog::OnInitDialog()
   {
     ......
     m_gdis[0].SetDestination(GetDlgItem(IDC_MAT_CONTROL1), true);
     m_gdis[1].SetDestination(GetDlgItem(IDC_MAT_CONTROL2), true);

     for (int i = 0; i < PK_MAX_MAT_CONTROL; i++) {
       m_mats[i].release();
     }
     ......
   }



   LRESULT Cmydialog::OnShowMat(WPARAM wParam, LPARAM lParam)
   {
     int id = (int)wParam;
     if (id < 0 || id >= PK_MAX_MAT_CONTROL)
       return -1;
    
     cv::Mat m = m_mats[id];
     if (m.data && m.cols && m.rows) {
       m_gdis[id].DrawImg(m);
     }
     return 0;
   }



显示（可以在后台线程执行）：

   m_mats[0] = frame;
   ::SendMessage(GetSafeHwnd(), WM_SHOW_MAT, 0, NULL);



3.目前只支持mfc


