---
title: GitHub Pages サイトのカスタムドメインを管理する
intro: '特定の DNS レコードとリポジトリ設定を設定または更新し、{% data variables.product.prodname_pages %} サイトのデフォルトドメインをカスタムドメインに指定することができます。'
redirect_from:
  - /articles/quick-start-setting-up-a-custom-domain
  - /articles/setting-up-an-apex-domain
  - /articles/setting-up-a-www-subdomain
  - /articles/setting-up-a-custom-domain
  - /articles/setting-up-an-apex-domain-and-www-subdomain
  - /articles/adding-a-cname-file-to-your-repository
  - /articles/setting-up-your-pages-site-repository
  - /articles/managing-a-custom-domain-for-your-github-pages-site
  - /github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site
product: '{% data reusables.gated-features.pages %}'
versions:
  fpt: '*'
  ghec: '*'
topics:
  - Pages
shortTitle: カスタムドメインの管理
---

リポジトリの管理者権限があるユーザは、{% data variables.product.prodname_pages %} サイトのカスタムドメインを設定できます。

## カスタムドメインの設定について

DNS プロバイダでカスタムドメインを設定する前に、必ず {% data variables.product.prodname_pages %} サイトをカスタムドメインに追加してください。 カスタムドメインを {% data variables.product.product_name %} に追加せずに DNS プロバイダに設定すると、別のユーザがあなたのサブドメインにサイトをホストできることになります。

{% windows %}

DNS レコードの設定が正しいかどうかを検証するために利用できる`dig` コマンドは、Windows には含まれていません。 DNS レコードが正しく設定されているかを検証する前に、[BIND](https://www.isc.org/bind/) をインストールする必要があります。

{% endwindows %}

{% note %}

**注釈:** DNS の変更が伝播するには、最大 24 時間かかります。

{% endnote %}

## サブドメインを設定する

`www` または `www.example.com` や `blog.example.com` などのカスタムサブドメインを設定するには、リポジトリ設定にドメインを追加する必要があります。 その後、DNS プロバイダで CNAME レコードを設定します。

{% data reusables.pages.navigate-site-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
4. "Custom domain（カスタムドメイン）" の下で、カスタムドメインを入力して**Save（保存）**をクリックします。 サイトをブランチから公開しているなら、これで`CNAME`ファイルをソースブランチのルートに追加するコミットが作成されます。 サイトをカスタムの{% data variables.product.prodname_actions %}ワークフローで公開しているなら、`CNAME`ファイルは作成されません。 公開ソースに関する詳しい情報については「[GitHub Pagesサイトの公開ソースの設定](/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)」を参照してください。 ![カスタムドメインの保存ボタン](/assets/images/help/pages/save-custom-subdomain.png)
5. お使いの DNS プロバイダにアクセスし、サブドメインがサイトのデフォルトドメインを指す `CNAME` レコードを作成します。 たとえば、サイトで `www.example.com` というサブドメインを使いたい場合、`www.example.com` が `<user>.github.io` を指す`CNAME` レコードを作成します。 Organization サイトで `www.anotherexample.com` というサブドメインを使用する場合、`www.anotherexample.com` が `<organization>.github.io` を指す`CNAME` レコードを作成します。 `CNAME` レコードは、リポジトリ名を除いて、常に`<user>.github.io` または `<organization>.github.io` を指している必要があります。 {% data reusables.pages.contact-dns-provider %} {% data reusables.pages.default-domain-information %}

{% indented_data_reference reusables.pages.wildcard-dns-warning spaces=3 %}
{% data reusables.command_line.open_the_multi_os_terminal %}
6. DNS レコードが正しくセットアップされたことを確認するには、 `dig` コマンドを使います。_WWW.EXAMPLE.COM_ は、お使いのサブドメインに置き換えてください。
```shell
    $ dig <em>WWW.EXAMPLE.COM</em> +nostats +nocomments +nocmd
    > ;<em>WWW.EXAMPLE.COM.</em>                     IN      A
    > <em>WWW.EXAMPLE.COM.</em>              3592    IN      CNAME   <em>YOUR-USERNAME</em>.github.io.
    > <em>YOUR-USERNAME</em>.github.io.      43192   IN      CNAME   <em> GITHUB-PAGES-SERVER </em>.
    > <em> GITHUB-PAGES-SERVER </em>.         22      IN      A       192.0.2.1
```
{% data reusables.pages.build-locally-download-cname %}
{% data reusables.pages.enforce-https-custom-domain %}

## Apexドメインを設定する

`example.com` などの Apex ドメインを設定するには、カスタムドメインをリポジトリ設定で構成し、DNS プロバイダで少なくとも 1 つの `ALIAS`、`ANAME`、または `A` レコードを設定する必要があります。

{% data reusables.pages.www-and-apex-domain-recommendation %} 詳しい情報については、「[サブドメインを設定する](#configuring-a-subdomain)」を参照してください。

{% data reusables.pages.navigate-site-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
4. "Custom domain（カスタムドメイン）" の下で、カスタムドメインを入力して**Save（保存）**をクリックします。 サイトをブランチから公開しているなら、これで`CNAME`ファイルをソースブランチのルートに追加するコミットが作成されます。 サイトをカスタムの{% data variables.product.prodname_actions %}ワークフローで公開しているなら、`CNAME`ファイルは作成されません。 公開ソースに関する詳しい情報については「[GitHub Pagesサイトの公開ソースの設定](/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)」を参照してください。 ![カスタムドメインの保存ボタン](/assets/images/help/pages/save-custom-apex-domain.png)
5. DNS プロバイダに移動し、`ALIAS`、`ANAME`、または `A` レコードを作成します。 IPv6サポートのために`AAAA`レコードを作成することもできます。 {% data reusables.pages.contact-dns-provider %}
    - `ALIAS`または`ANAME`レコードを作成するには、Apexドメインをサイトのデフォルトドメインにポイントします。 {% data reusables.pages.default-domain-information %}
    - `A` レコードを作成するには、Apex ドメインが {% data variables.product.prodname_pages %} の IP アドレスを指すようにします。
      ```shell
      185.199.108.153
      185.199.109.153
      185.199.110.153
      185.199.111.153
      ```
    - `AAAA` レコードを作成するには、Apex ドメインが {% data variables.product.prodname_pages %} の IP アドレスを指すようにします。
      ```shell
      2606:50c0:8000::153
      2606:50c0:8001::153
      2606:50c0:8002::153
      2606:50c0:8003::153
      ```

{% indented_data_reference reusables.pages.wildcard-dns-warning spaces=3 %}
{% data reusables.command_line.open_the_multi_os_terminal %}
6. DNS レコードが正しく設定されたことを確認するには、 `dig` コマンドを使います。_EXAMPLE.COM_ は、お使いの Apex ドメインに置き換えてください。 結果が、上記の {% data variables.product.prodname_pages %} の IP アドレスに一致することを確認します。
   - `A`レコードの場合。
    ```shell
    $ dig <em>EXAMPLE.COM</em> +noall +answer -t A
    > <em>EXAMPLE.COM</em>     3600    IN A     185.199.108.153
    > <em>EXAMPLE.COM</em>     3600    IN A     185.199.109.153
    > <em>EXAMPLE.COM</em>     3600    IN A     185.199.110.153
    > <em>EXAMPLE.COM</em>     3600    IN A     185.199.111.153
    ```
   - `AAAA`レコードの場合。
    ```shell
    $ dig <em>EXAMPLE.COM</em> +noall +answer -t AAAA
    > <em>EXAMPLE.COM</em>     3600    IN AAAA     2606:50c0:8000::153
    > <em>EXAMPLE.COM</em>     3600    IN AAAA     2606:50c0:8001::153
    > <em>EXAMPLE.COM</em>     3600    IN AAAA     2606:50c0:8002::153
    > <em>EXAMPLE.COM</em>     3600    IN AAAA     2606:50c0:8003::153
    ```
{% data reusables.pages.build-locally-download-cname %}
{% data reusables.pages.enforce-https-custom-domain %}

## Apexドメインと`www`サブドメイン付きのドメインの設定

Apexドメインを使う場合、コンテンツをApexドメインと`www`サブドメイン付きのドメインの双方でホストするよう{% data variables.product.prodname_pages %}サイトを設定することをおすすめします。

Apexドメインと共に`www`サブドメインをセットアップするには、DNSプロバイダで`ALIAS`、`ANAME`、`A`のいずれかのレコードが作成することによって、まずApexドメインを設定しします。 詳しい情報については「[Apexドメインの設定](#configuring-an-apex-domain)」を参照してください。

Apexドメインを設定したら、DNSプロバイダでCNAMEレコードを設定しなければなりません。

1. DNSプロバイダにアクセスして、`www.example.com`がサイトのデフォルトドメインの`<user>.github.io`もしくは`<organization>.github.io`を指すようにする`CNAME`レコードを作成してください。 リポジトリ名は含めないでください。 {% data reusables.pages.contact-dns-provider %} {% data reusables.pages.default-domain-information %}
2. DNS レコードが正しくセットアップされたことを確認するには、 `dig` コマンドを使います。_WWW.EXAMPLE.COM_ は、`www`サブドメイン付きのドメインに置き換えてください。
```shell
    $ dig <em>WWW.EXAMPLE.COM</em> +nostats +nocomments +nocmd
    > ;<em>WWW.EXAMPLE.COM.</em>                     IN      A
    > <em>WWW.EXAMPLE.COM.</em>              3592    IN      CNAME   <em>YOUR-USERNAME</em>.github.io.
    > <em>YOUR-USERNAME</em>.github.io.      43192   IN      CNAME   <em> GITHUB-PAGES-SERVER </em>.
    > <em> GITHUB-PAGES-SERVER </em>.         22      IN      A       192.0.2.1
```
## カスタムドメインの削除

{% data reusables.pages.navigate-site-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
4. "Custom domain（カスタムドメイン）"の下で、**Remove（削除）**をクリックしてください。 ![カスタムドメインの保存ボタン](/assets/images/help/pages/remove-custom-domain.png)

## カスタムドメインの保護

{% data reusables.pages.secure-your-domain %} 詳しい情報については「[{% data variables.product.prodname_pages %}のカスタムドメインの検証](/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)」を参照してください。

## 参考リンク

- [カスタムドメインと {% data variables.product.prodname_pages %} のトラブルシューティング](/articles/troubleshooting-custom-domains-and-github-pages)
