# Azure AI Search Kurulum Kılavuzu

Bu kılavuz, Azure portalını kullanarak Azure AI Search'ü nasıl kuracağınızı ve yapılandıracağınızı gösterecektir.

## Ön Koşullar

Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- Azure aboneliği. Eğer Azure aboneliğiniz yoksa, [Azure Ücretsiz Hesap](https://azure.microsoft.com/free/?wt.mc_id=studentamb_258691) oluşturabilirsiniz.

## Adım 1: Azure AI Search Hizmeti Oluşturma

1. [Azure portalına](https://portal.azure.com/?wt.mc_id=studentamb_258691) giriş yapın.
2. Sol taraftaki gezinme panosunda **Kaynak oluştur**'a tıklayın.
3. Arama kutusuna "Azure Cognitive Search" yazın ve sonuçlar listesinden **Azure Cognitive Search**'ü seçin.
4. **Oluştur** düğmesine tıklayın.
5. **Temel Bilgiler** sekmesinde aşağıdaki bilgileri girin:
   - **Abonelik**: Azure aboneliğinizi seçin.
   - **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.
   - **Kaynak adı**: Arama hizmetiniz için benzersiz bir ad girin.
   - **Bölge**: Kullanıcılarınıza en yakın bölgeyi seçin.
   - **Fiyatlandırma katmanı**: İhtiyaçlarınıza uygun bir fiyatlandırma katmanı seçin. Test için Ücretsiz katmanı kullanabilirsiniz.
6. **Gözden geçir + oluştur**'a tıklayın.
7. Ayarları gözden geçirin ve arama hizmetini oluşturmak için **Oluştur**'a tıklayın.

## Adım 2: Azure AI Search ile Başlarken

1. Dağıtım tamamlandıktan sonra, Azure portalında arama hizmetinize gidin.
2. Arama hizmeti genel bakış sayfasında **Hızlı başlangıç** düğmesine tıklayın.
3. Hızlı başlangıç kılavuzundaki adımları takip ederek bir dizin oluşturun, veri yükleyin ve arama sorgusu gerçekleştirin.

### Dizin Oluşturma

1. Hızlı başlangıç kılavuzunda **Dizin oluştur**'a tıklayın.
2. Alanları ve özelliklerini (veri tipi, aranabilir, filtrelenebilir gibi) belirterek dizin şemasını tanımlayın.
3. Dizini oluşturmak için **Oluştur**'a tıklayın.

### Veri Yükleme

1. Hızlı başlangıç kılavuzunda **Veri yükle**'ye tıklayın.
2. Bir veri kaynağı seçin (örneğin, Azure Blob Depolama, Azure SQL Veritabanı) ve gerekli bağlantı bilgilerini girin.
3. Veri kaynağı alanlarını dizin alanlarıyla eşleştirin.
4. Verileri dizine yüklemek için **Gönder**'e tıklayın.

### Arama Sorgusu Gerçekleştirme

1. Hızlı başlangıç kılavuzunda **Arama gezgini**'ne tıklayın.
2. Arama işlevini test etmek için arama kutusuna bir sorgu girin.
3. Arama sonuçlarını gözden geçirin ve gerektiğinde dizin şemasını veya verileri düzenleyin.

## Adım 3: Azure AI Search Araçlarını Kullanma

Azure AI Search, arama yeteneklerinizi geliştirmek için çeşitli araçlarla entegre olur. Gelişmiş yapılandırmalar ve işlemler için Azure CLI, Python SDK ve diğer araçları kullanabilirsiniz.

### Azure CLI Kullanımı

1. [Azure CLI Kurulumu](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?wt.mc_id=studentamb_258691) sayfasındaki talimatları takip ederek Azure CLI'yi yükleyin.
2. Azure CLI'de oturum açmak için aşağıdaki komutu kullanın:
   ```bash
   az login
   ```
3. Azure CLI kullanarak yeni bir arama hizmeti oluşturun:
   ```bash
   az search service create --resource-group <kaynak-grubu> --name <hizmet-adı> --sku Free
   ```
4. Azure CLI kullanarak bir dizin oluşturun:
   ```bash
   az search index create --service-name <hizmet-adı> --name <dizin-adı> --fields "alan1:tip, alan2:tip"
   ```

### Python SDK Kullanımı

1. Python için Azure Cognitive Search istemci kütüphanesini yükleyin:
   ```bash
   pip install azure-search-documents
   ```
2. Dizin oluşturmak ve belgeleri yüklemek için aşağıdaki Python kodunu kullanın:
   ```python
   from azure.core.credentials import AzureKeyCredential
   from azure.search.documents import SearchClient
   from azure.search.documents.indexes import SearchIndexClient
   from azure.search.documents.indexes.models import SearchIndex, SimpleField, edm

   service_endpoint = "https://<service-name>.search.windows.net"
   api_key = "<api-key>"

   index_client = SearchIndexClient(service_endpoint, AzureKeyCredential(api_key))

   fields = [
       SimpleField(name="id", type=edm.String, key=True),
       SimpleField(name="content", type=edm.String, searchable=True),
   ]

   index = SearchIndex(name="sample-index", fields=fields)

   index_client.create_index(index)

   search_client = SearchClient(service_endpoint, "sample-index", AzureKeyCredential(api_key))

   documents = [
       {"id": "1", "content": "Hello world"},
       {"id": "2", "content": "Azure Cognitive Search"}
   ]

   search_client.upload_documents(documents)
   ```

Daha detaylı bilgi için aşağıdaki dokümantasyonlara başvurabilirsiniz:

- [Azure Cognitive Search hizmeti oluşturma](https://learn.microsoft.com/en-us/azure/search/search-create-service-portal?wt.mc_id=studentamb_258691)
- [Azure Cognitive Search ile başlarken](https://learn.microsoft.com/en-us/azure/search/search-get-started-portal?wt.mc_id=studentamb_258691)
- [Azure AI Search Araçları](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/azure-ai-search?tabs=azurecli%2Cpython&pivots=code-examples?wt.mc_id=studentamb_258691)

## Sonuç

Azure portalını ve entegre araçları kullanarak Azure AI Search'ü başarıyla kurdunuz. Artık arama çözümlerinizi geliştirmek için Azure AI Search'ün daha gelişmiş özelliklerini ve yeteneklerini keşfedebilirsiniz.

Daha fazla yardım için [Azure Cognitive Search dokümantasyonunu](https://learn.microsoft.com/en-us/azure/search/?wt.mc_id=studentamb_258691) ziyaret edebilirsiniz.
