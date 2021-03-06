---
title: "Azure Stack の ID の概要 | Microsoft Docs"
description: "Azure Stack で使用できる ID システムについて説明します。"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Azure Stack の ID の概要

Azure Stack には、Azure AD、または ID プロバイダーとして Active Directory (AD) を使用する Active Directory フェデレーション サービス (AD FS) が必要です。 以下の概念と承認についての詳細情報は、ID プロバイダーの選択に役立ちます。 プロバイダーの選択は、Azure Stack を初めてデプロイするときに、一度だけ行います。  

Azure AD にするか AD FS にするかの選択は、Azure Stack をデプロイするモードによって制限されることがあります。 
- 接続モードでデプロイする場合は、Azure AD または AD FS を使用できます。 
- インターネットに接続せずに切断モードでデプロイする場合は、AD FS のみがサポートされます。

これらの選択の詳細については、Azure Stack の環境に応じて、以下の記事を参照してください。
- Azure Stack デプロイ キットの場合は、「[ID に関する考慮事項](azure-stack-datacenter-integration.md#identity-considerations)」を参照してください。
- Azure Stack 統合システムの場合は、[Azure Stack 統合システムに対するデプロイ計画の決定](azure-stack-deployment-decisions.md)に関するページを参照してください。

 
## <a name="common-concepts-for-identity"></a>ID に関する一般的な概念
以下のセクションでは、ID プロバイダーの一般的な概念と、Azure Stack での使用について説明します。
![ID プロバイダーに関する用語](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>ディレクトリ テナントと組織
ディレクトリとは、*ユーザー*、*アプリケーション*、*グループ*、および*サービス プリンシパル*に関する情報を保持するコンテナーのことです。
 
ディレクトリ テナントとは、Microsoft や自分の会社のような、"*組織*" のことです。 
- Azure AD では複数のテナントがサポートされており、複数の組織をそれぞれ独自のディレクトリでサポートすることができます。 Azure AD を使用し、複数のテナントがある場合、1 つのテナントのアプリケーションとユーザーに、同じディレクトリの他のテナントへのアクセス権を与えることができます。
- AD FS では、単一のテナントのみがサポートされます。そのため、単一の組織のみがサポートされます。 

### <a name="users-and-groups"></a>ユーザーとグループ
ユーザー アカウント (ID) は、ユーザー ID とパスワードを使用して個人を認証する標準的なアカウントです。 グループには、ユーザーまたは他のグループを含めることができます。 

ユーザーとグループを作成および管理する方法は、使用する ID ソリューションによって異なります。 

Azure Stack では、ユーザー アカウントに次のような特徴があります。 
- *username@domain* という形式で作成されます。 AD FS はユーザー アカウントを Active Directory にマッピングしますが、AD FS では "_&lt;ドメイン>\<エイリアス>_" 形式の使用がサポートされていません。 
- 多要素認証を使用するようにセットアップすることができます。 
- 最初に登録するディレクトリ、つまり組織のディレクトリに制限されています。
- オンプレミスのディレクトリからインポートすることができます。 詳細については、Azure ドキュメントの「[オンプレミスのディレクトリと Azure Active Directory の統合](/azure/active-directory/connect/active-directory-aadconnect)」を参照してください。  

組織のテナント ポータルにログインするときは、https://portal.local.azurestack.external を使用します。 

### <a name="guest-users"></a>ゲスト ユーザー
ゲスト ユーザーとは、あるディレクトリ内のリソースへのアクセスを許可されている、他のディレクトリ テナントのユーザー アカウントのことです。 ゲスト ユーザーをサポートするには、Azure AD を使用し、マルチテナントのサポートを有効にする必要があります。 サポートされている場合は、自社のディレクトリ テナント内のリソースにアクセスするようにゲスト ユーザーを招待することができます。これにより、外部組織との共同作業が可能になります。 

ゲスト ユーザーを招待するには、クラウド オペレーターおよびユーザーが [Azure AD B2B コラボレーション](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)を使用できます。 招待されたユーザーは、ディレクトリのドキュメント、リソース、およびアプリケーションにアクセスできるようになりますが、リソースおよびデータに対する制御は元の管理者が保持し続けます。 

ゲスト ユーザーは、他の組織のディレクトリ テナントにログインすることができます。 そうするには、その組織のディレクトリ名をポータル URL に追加する必要があります。  たとえば、contoso.com に属していて、Fabrikam ディレクトリにログインする場合は、https://portal.local.azurestack.external/fabrikam.onmicrosoft.com を使用します。

### <a name="applications"></a>[アプリケーション]
Azure AD または AD FS にアプリケーションを登録し、組織内のユーザーに提供することができます。 

アプリケーションとして、次のものがあります。
- **Web アプリケーション** – たとえば、Azure Portal、Azure Resource Manager などです。  これらでは、Web API 呼び出しがサポートされています。 
- **ネイティブ クライアント** – たとえば、Azure PowerShell、Visual Studio、Azure コマンド ライン インターフェイス (CLI) などです。

アプリケーションでは、次の 2 種類のテナントをサポートすることができます。 
- **シングルテナント** アプリケーションは、アプリケーションが登録されているのと同じディレクトリのユーザーとサービスだけをサポートします。 

  > [!NOTE]    
  > AD FS では単一のディレクトリしかサポートされないため、AD FS トポロジで作成するアプリケーションは、設計上、シングルテナント アプリケーションです。
- **マルチテナント** アプリケーションは、アプリケーションが登録されているディレクトリと追加のテナント ディレクトリの、両方のユーザーとサービスをサポートします。  マルチテナント アプリケーションでは、別のテナント ディレクトリ (別の Azure AD テナント) のユーザーがアプリケーションにサインインすることができます。     

  マルチテナントの詳細については、[マルチテナントの有効化](azure-stack-enable-multitenancy.md)に関するページを参照してください。  

  マルチテナント アプリの開発の詳細については、[マルチテナント アプリ](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)に関するページを参照してください。

アプリケーションを登録するときに、次の 2 つのオブジェクトが作成されます。
- **アプリケーション オブジェクト** – アプリケーション オブジェクトは、すべてのテナントにわたる、アプリケーションのグローバルな表現です。 ソフトウェア アプリケーションと 1 対 1 の関係であり、アプリケーションが最初に登録されたディレクトリだけに存在します。

 
   
- **サービス プリンシパル オブジェクト** – サービス プリンシパルは、アプリケーションが最初に登録されたディレクトリ内でそのアプリケーションのために作成される資格情報です。 また、サービス プリンシパルは、そのアプリケーションが使用される追加の各テナントのディレクトリにも作成されます。 ソフトウェア アプリケーションと 1 対多の関係にすることができます。   

アプリケーション オブジェクトとサービス プリンシパル オブジェクトの詳細については、「[Azure Active Directory のアプリケーション オブジェクトとサービス プリンシパル オブジェクト (Azure AD)](/azure/active-directory/develop/active-directory-application-objects)」を参照してください。 

### <a name="service-principals"></a>サービス プリンシパル 
サービス プリンシパルは、Azure Stack 内のリソースへのアクセスを許可する、アプリケーションまたはサービスの*資格情報*のセットです。 サービス プリンシパルを使用すると、アプリケーションのアクセス許可と、アプリケーションを使用するユーザーのアクセス許可が分離されます。

サービス プリンシパルは、アプリケーションが使用される各テナントに作成されます。 そのテナントによって保護されているリソース (ユーザーなど) へのサインインとアクセスのために、サービス プリンシパルは ID を確立します。   

- シングルテナント アプリケーションには、サービス プリンシパルが、最初に作成されたディレクトリに 1 つしかありません。  このサービス プリンシパルは、アプリケーションの登録時に作成され、使用が承認されます。 
- マルチテナント Web アプリケーションまたは API には、アプリケーションの使用を承認されたユーザーの各テナントで作成されたサービス プリンシパルがあります。  

サービス プリンシパルの資格情報は、キー (Azure Portal を通じて生成) または証明書です。  証明書は、キーを使用するよりも安全であると考えられるため、自動化に適しています。 


> [!NOTE]    
> Azure Stack で AD FS を使用する場合、管理者だけがサービス プリンシパルを作成できます。 AD FS では、サービス プリンシパルは証明書を必要とし、特権エンドポイント (PEP) を通じて作成されます。 詳細については、「[Azure Stack へのアクセスをアプリケーションに提供する](azure-stack-create-service-principals.md)」を参照してください。

Azure Stack のサービス プリンシパルについては、[サービス プリンシパルの作成](azure-stack-create-service-principals.md)に関するページを参照してください。


### <a name="services"></a>サービス
ID プロバイダーと対話する Azure Stack のサービスは、ID プロバイダーにアプリケーションとして登録されます。 アプリケーションと同様に、登録によってサービスは ID システムで認証できるようになります。 

すべての Azure サービスは、[OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) プロトコルと [JSON Web トークン](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) を使用して ID を確立します。 Azure AD と AD FS でプロトコルの使用に一貫性があるため、Azure [Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) を使用して、オンプレミスまたは Azure (接続シナリオの場合) で認証することができます。 ADAL を使用すると、クロスクラウドおよびオンプレミスのリソース管理に Azure PowerShell や CLI などのツールを使用することもできます。 


### <a name="identities-and-your-identity-system"></a>ID と ID システム 
Azure Stack の ID には、ユーザー アカウント、グループ、およびサービス プリンシパルが含まれます。 Azure Stack をインストールすると、いくつかの組み込みのアプリケーションとサービスが、ディレクトリ テナント内の ID プロバイダーに自動的に登録されます。 登録される一部のサービスは、管理用に使用されます。 その他のサービスは、ユーザーが使用できます。 既定の登録では、コア サービスに、相互に対話できる ID が与えられます。後で追加する ID もあります。
Azure AD をマルチテナンシーでセットアップすると、一部のアプリケーションが新しいディレクトリにも登録されます。  

## <a name="authentication-and-authorization"></a>認証と権限承認
 

### <a name="authentication-by-applications-and-users"></a>アプリケーションとユーザーによる認証
  
![Azure Stack のレイヤー間の ID](media/azure-stack-identity-overview/identity-layers.png)

アプリケーションとユーザーにとって、Azure Stack のアーキテクチャは 4 つのレイヤーで表されます。 各レイヤー間の対話には、さまざまな種類の認証を使用できます。


|レイヤー    |レイヤー間の認証  |
|---------|---------|
|管理ポータルなどのツールとクライアント     | Azure Stack のリソースにアクセスしたりそれを変更したりするために、ツールとクライアントは [JSON Web トークン](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) を使用して Azure Resource Manager を呼び出します。  <br><br> Azure Resource Manager は、発行されたトークンの "*要求*" で JWT とピークを検証して、ユーザーまたはサービス プリンシパルが Azure Stack で持つ承認のレベルを見積もります。        |
|Azure Resource Manager とそのコア サービス     |Azure Resource Manager は、リソース プロバイダーと通信して、ユーザーからの通信を転送します。 <br><br> 転送では、[Azure Resource Manager テンプレート](/azure/azure-stack/user/azure-stack-arm-templates.md)を通じて、"*直接命令*" 呼び出しまたは "*宣言*" 呼び出しが使用されます。|
|リソース プロバイダー     |リソース プロバイダーに渡された呼び出しは、証明書ベースの認証で保護されます。 <br><br>Azure Resource Manager とリソース プロバイダーは、API を介した通信を継続します。 Azure Resource Manager から受信したすべての呼び出しを、リソース プロバイダーはその証明書で検証します。|
|インフラストラクチャとビジネス ロジック     |リソース プロバイダーは、任意の認証モードを使用して、ビジネス ロジックおよびインフラストラクチャと通信します。 Azure Stack 付属の既定のリソース プロバイダーは、この通信を保護するために Windows 認証を使用します。|

![認証に必要な情報](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Azure Resource Manager への認証
ID プロバイダーで認証して JWT を受け取るには、次の情報が必要です。 
1.  **ID システム (機関) の URL** - ID プロバイダーに到達できる URL。 たとえば、*&lt;https://login.windows.net>*。 
2.  **Azure Resource Manager のアプリ ID URI** - ID プロバイダーに登録され、各 Azure Stack インストールに固有の、Azure Resource Manager の一意の識別子。
3.  **資格情報** – ID プロバイダーでの認証に使用している資格情報。  
4.  **Azure Resource Manager の URL** – URL は、Azure Resource Manager サービスの場所です。 たとえば、*&lt;https://management.azure.com>* または *&lt;https://management.local.azurestack.external>* です。

プリンシパル (クライアント、アプリケーション、またはユーザー) がリソースにアクセスするために認証要求を行う場合、その要求には以下のものが含まれている必要があります。
- プリンシパルの資格情報。
- アクセスするリソースのアプリ ID URI。  

資格情報は、ID プロバイダーによって検証されます。 ID プロバイダーは、App ID URI が登録済みアプリケーションのものであることと、プリンシパルがそのリソースのトークンを取得するための正しい特権を持っていることも検証します。 要求が有効な場合は、JSON Web トークンが付与されます。 

その後、トークンは要求のヘッダーを Azure Resource Manager に渡す必要があります。 Azure Resource Manager は、不特定の順序で以下の検証を行います。
- トークンが正しい ID プロバイダーからのものであることを確認するために、*issuer* (iss) 要求を検証します。 
- トークンが Azure Resource Manager に発行されたことを確認するために、*audience* (aud) 要求を検証します。 
- JWT が OpenID を通じて構成された証明書で署名されていることと、それが Azure Resource Manager に認識されていることを検証します。 
- トークンがアクティブであり、承認可能であることを確認するために、*issued at* (iat) および *expiry* (exp) 要求を見直します。 

すべての検証が完了すると、Azure Resource Manager は *objected* (oid) および *groups* 要求を使用して、プリンシパルがアクセスできるリソースの一覧を作成します。 

![トークン交換プロトコルの図](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>ロールベースのアクセス制御を使用する  
Azure Stack のロールベースのアクセス制御 (RBAC) は、Microsoft Azure での実装と一貫しています。  適切な RBAC ロールをユーザー、グループ、およびアプリケーションに割り当てることによって、リソースへのアクセスを管理することができます。 Azure Stack で RBAC を使用する方法については、以下の記事を参照してください。
- [Azure Portal でのロールベースの Access Control の基礎を確認する](/azure/active-directory/role-based-access-control-what-is)
- [ロールベースのアクセス制御を使用して Azure サブスクリプション リソースへのアクセスを管理する](/azure/active-directory/role-based-access-control-configure)
- [Azure のロールベースのアクセス制御のためのカスタム ロールを作成する](/azure/active-directory/role-based-access-control-custom-roles)
- Azure Stack での[ロール ベースのアクセス制御の管理](azure-stack-manage-permissions.md)


### <a name="authenticate-with-azure-powershell"></a>Azure PowerShell で認証する  
Azure PowerShell を使用して Azure Stack で認証する方法の詳細については、「[Azure Stack ユーザーの PowerShell 環境の構成](azure-stack-powershell-configure-user.md)」を参照してください。

### <a name="authenticate-with-azure-cli"></a>Azure CLI で認証する
Azure PowerShell を使用して Azure Stack で認証する方法の詳細については、「[Azure Stack で使用する CLI をインストールして構成する](/azure/azure-stack/user/azure-stack-connect-cli.md)」を参照してください。

## <a name="next-steps"></a>次の手順
- [ID アーキテクチャ](azure-stack-identity-architecture.md)   
- [データセンターの統合 - ID](azure-stack-integrate-identity.md)




