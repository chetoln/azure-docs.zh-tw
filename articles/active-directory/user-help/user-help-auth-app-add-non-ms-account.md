---
title: 將非 Microsoft 帳戶新增至 Microsoft Authenticator 應用程式 - Azure Active Directory | Microsoft Docs
description: 如何將非 Microsoft 帳戶 (例如 Google、Facebook 或 GitHub 的帳戶) 新增至 Microsoft Authenticator 應用程式以進行雙因素驗證。
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6f94ba30c06fc6975ab212c895cecefe5d383fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473901"
---
# <a name="add-your-non-microsoft-accounts"></a>新增您的非 Microsoft 帳戶
將非 Microsoft 帳戶 (例如 Google、Facebook 或 GitHub 的帳戶) 新增至 Microsoft Authenticator 應用程式以進行雙因素驗證。 Microsoft Authenticator 應用程式適用於任何使用雙因素驗證的應用程式，以及任何支援限時單次密碼 (TOTP) 標準的帳戶。

>[!Important]
>您必須先下載並安裝 Microsoft Authenticator 應用程式，才可新增帳戶。 如果尚未這麼做，請依照[下載並安裝應用程式](user-help-auth-app-download-install.md)文章中的步驟。

## <a name="add-personal-accounts"></a>新增個人帳戶
對於您所有的個人帳戶，您通常必須：

1. 登入您的帳戶，然後使用您的裝置或電腦開啟雙因素驗證。

2. 將帳戶新增至 Microsoft Authenticator 應用程式。 系統可能會要求您掃描 QR 代碼作為此程序的一部分。

我們在此提供的是 Facebook、Google、GitHub 及 Amazon 帳戶的相關程序，但此程序也適用於任何其他應用程式，例如 Instagram、Netflix 或 Adobe。

## <a name="add-your-google-account"></a>新增您的 Google 帳戶
藉由開啟雙因素驗證，然後將帳戶新增至應用程式，以新增您的 Google 帳戶。

### <a name="turn-on-two-factor-verification"></a>開啟雙因素驗證

1. 在您的電腦上移至 https://myaccount.google.com/signinoptions/two-step-verification/enroll-welcome，並選取 [開始使用]，然後驗證您的身分識別。

2. 依照頁面上的步驟，為您的個人 Google 帳戶開啟雙步驟驗證。

### <a name="add-your-google-account-to-the-app"></a>將您的 Google 帳戶新增至應用程式

1. 在電腦的 Google 頁面上移至 [設定替代的第二個步驟] 區段，並從 [Authenticator 應用程式] 區段中選擇 [設定]。

2. 在 [從 Authenticator 應用程式取得驗證碼] 頁面上，根據您的電話類型選取 [Android] 或 [iPhone]，然後選取 [下一步]。

    您會取得一個 QR 代碼，此代碼可用來自動將您的帳戶與 Microsoft Authenticator 應用程式產生關聯。 請勿關閉此視窗。

3. 開啟 Microsoft Authenticator 應用程式，從右上角的 [自訂和控制] 圖示中選取 [新增帳戶]，然後選取 [其他帳戶 (Google、Facebook 等等)]。

4. 使用裝置相機掃描電腦上的 [設定 Authenticator] 頁面中顯示的 QR 代碼。

    >[!Note]
    >如果相機無法正常運作，您可以手動輸入 QR 代碼和 URL。

5. 在您的裝置上檢閱 Microsoft Authenticator 應用程式的 [帳戶] 頁面，以確定您的帳戶資訊正確無誤，並且有相關聯的六位數驗證碼。

    為了提高安全性，驗證碼每隔 30 秒就會變更，避免多次使用同一個驗證碼。

6. 在您的電腦上選取 [設定 Authenticator] 頁面上的 [下一步]，為您的 Google 帳戶輸入應用程式提供的六位數驗證碼，然後選取 [驗證]。

7. 帳戶經過驗證後，您可以選取 [完成] 以關閉 [設定 Authenticator] 頁面。

    >[!NOTE]
    >如需更多雙因素驗證和 Google 帳戶的相關資訊，請參閱[開啟雙步驟驗證](https://support.google.com/accounts/answer/185839)和[深入了解雙步驟驗證](https://www.google.com/landing/2step/help.html)。

## <a name="add-your-facebook-account"></a>新增您的 Facebook 帳戶
藉由開啟雙因素驗證，然後將帳戶新增至應用程式，以新增您的 Facebook 帳戶。

### <a name="turn-on-two-factor-verification"></a>開啟雙因素驗證

1. 在您的電腦上開啟 Facebook，並選取右上角的下拉式功能表，然後移至 [設定] > [安全性和登入]。

    [安全性和登入] 頁面隨即出現。

2. 向下移至 [雙因素驗證] 區段中的 [使用雙因素驗證] 選項，然後選取 [編輯]。

    [雙因素驗證] 頁面隨即出現。

3. 選取 [開啟]。

### <a name="add-your-facebook-account-to-the-app"></a>將您的 Facebook 帳戶新增至應用程式

1. 在電腦上的 Facebook 頁面中移至 [新增備份] 區段，然後從 [驗證應用程式] 區域中選擇 [設定]。

    您會取得一個 QR 代碼，此代碼可用來自動將您的帳戶與 Microsoft Authenticator 應用程式產生關聯。 請勿關閉此視窗。

2. 開啟 Microsoft Authenticator 應用程式，從右上角的 [自訂和控制] 圖示中選取 [新增帳戶]，然後選取 [其他帳戶 (Google、Facebook 等等)]。

3. 使用裝置相機掃描電腦上的 [雙因素驗證] 頁面中顯示的 QR 代碼。

    >[!Note]
    >如果相機無法正常運作，您可以手動輸入 QR 代碼和 URL。

4. 在您的裝置上檢閱 Microsoft Authenticator 應用程式的 [帳戶] 頁面，以確定您的帳戶資訊正確無誤，並且有相關聯的六位數驗證碼。

    為了提高安全性，驗證碼每隔 30 秒就會變更，避免多次使用同一個驗證碼。

5. 在電腦的 [雙因素驗證] 頁面上選取 [下一步]，然後為您的 Facebook 帳戶輸入應用程式提供的六位數驗證碼。

    帳戶經過驗證後，您即可使用應用程式來驗證您的帳戶。

    >[!NOTE]
    >如需更多雙因素驗證和您 Facebook 帳戶的更多資訊，請參閱[什麼是雙重驗證，此機制如何運作？](https://www.facebook.com/help/148233965247823)。

## <a name="add-your-github-account"></a>新增 GitHub 帳戶
藉由開啟雙因素驗證，然後將帳戶新增至應用程式，以新增您的 GitHub 帳戶。

### <a name="turn-on-two-factor-verification"></a>開啟雙因素驗證

1. 在您的電腦上開啟 GitHub，並從右上角選取您的影像，然後選取 [設定]。

    [雙因素驗證] 頁面隨即出現。

2. 從 [個人設定] 提要欄位中選取 [安全性]，然後從 [雙因素驗證] 區域中選取 [啟用雙因素驗證]。

### <a name="add-your-github-account-to-the-app"></a>將您的 GitHub 帳戶新增至應用程式

1. 在電腦的 [雙因素驗證] 頁面上，選取 [使用應用程式進行設定]。

2. 儲存您的復原碼以便在失去存取權時恢復您的帳戶，然後按 [下一步]。 

    您可以將復原碼下載到您的裝置、列印實體複本，或將其複製到密碼管理員工具中，以儲存復原碼。

3. 在 [雙因素驗證] 頁面上，選取 [使用應用程式進行設定]。

    頁面會變更以顯示 QR 代碼。 請勿關閉此頁面。

4. 開啟 Microsoft Authenticator 應用程式，從右上角的 [自訂和控制] 圖示中選取 [新增帳戶]，再選取 [其他帳戶 (Google、Facebook 等等)]，然後從位於頁面頂端的文字中選取 [輸入此文字碼]。

    Microsoft Authenticator 應用程式無法掃描 QR 代碼，因此您必須以手動方式輸入代碼。

5. 輸入**帳戶名稱** (例如 GitHub)，並輸入步驟 4 中的**祕密金鑰**，然後選取 [完成]。

4. 在電腦的 [雙因素 Authenticator] 頁面上，為您的 GitHub 帳戶輸入應用程式提供的六位數驗證碼，然後選取 [啟用]。

    應用程式的 [帳戶] 頁面會顯示您的帳戶名稱和六位數的驗證碼。 為了提高安全性，驗證碼每隔 30 秒就會變更，避免多次使用同一個驗證碼。

    >[!NOTE]
    >如需關於雙因素驗證和 GitHub 帳戶的相關資訊，請參閱[關於雙因素驗證](https://help.github.com/articles/about-two-factor-authentication/)。

## <a name="add-your-amazon-account"></a>新增您的 Amazon 帳戶
藉由開啟雙因素驗證，然後將帳戶新增至應用程式，以新增您的 Amazon 帳戶。

### <a name="turn-on-two-factor-verification"></a>開啟雙因素驗證

1. 在您的電腦上開啟 Amazon，並選取 [帳戶和清單] 下拉式功能表，然後選取 [您的帳戶]。

2. 選取 [登入和安全性]，並登入您的 Amazon 帳戶，然後選取 [進階安全性設定] 區域中的 [編輯]。

    [進階安全性設定] 頁面隨即出現。

3. 選取 **[馬上開始]** 。

4. 從 [選擇接收代碼的方式] 頁面中選取 [Authenticator 應用程式]。

    頁面會變更以顯示 QR 代碼。 請勿關閉此頁面。

5. 開啟 Microsoft Authenticator 應用程式，從右上角的 [自訂和控制] 圖示中選取 [新增帳戶]，然後選取 [其他帳戶 (Google、Facebook 等等)]。

6. 使用裝置相機掃描電腦的 [選擇接收代碼的方式] 頁面中顯示的 QR 代碼。

    >[!Note]
    >如果相機無法正常運作，您可以手動輸入 QR 代碼和 URL。

5. 在您的裝置上檢閱 Microsoft Authenticator 應用程式的 [帳戶] 頁面，以確定您的帳戶資訊正確無誤，並且有相關聯的六位數驗證碼。

    為了提高安全性，驗證碼每隔 30 秒就會變更，避免多次使用同一個驗證碼。

6. 在電腦的 [選擇接收代碼的方式] 頁面上，為您的 Amazon 帳戶輸入應用程式提供的六位數驗證碼，然後選取 [驗證代碼並繼續]。

7. 完成其餘註冊程序，包括新增備份驗證方法 (例如簡訊)，然後選取 [傳送代碼]。

8. 在您電腦的 [新增備份驗證方法] 頁面上，為您的 Amazon 帳戶輸入備份驗證方法所提供的六位數驗證碼，然後選取 [驗證代碼並繼續]。

9. 在 快要完成了 頁面上，決定是否要讓您的電腦成為信任的裝置，然後選取 我知道了。

    [進階安全性設定] 頁面隨即出現，顯示更新的雙因素驗證詳細資料。

    >[!NOTE]
    >如需更多雙因素驗證和 Amazon 帳戶的相關資訊，請參閱[關於雙步驟驗證](https://www.amazon.com/gp/help/customer/display.html?nodeId=201596330)和[使用雙步驟驗證登入](https://www.amazon.com/gp/help/customer/display.html?nodeId=201962440)。


## <a name="next-steps"></a>後續步驟

- 將您的帳戶新增至應用程式之後，您即可在裝置上使用 Authenticator 應用程式登入。 如需詳細資訊，請參閱[使用應用程式登入](user-help-auth-app-sign-in.md)。

- 對於執行 iOS 的裝置，您也可以將您的帳戶認證和相關應用程式設定 (例如您的帳戶順序) 備份到雲端。 如需更多資訊，請參閱[使用 Microsoft Authenticator 應用程式備份和復原](user-help-auth-app-backup-recovery.md)。
