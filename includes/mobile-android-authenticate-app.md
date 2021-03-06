
1. Android Studio でプロジェクトを開きます。

2. Android Studio の **Project Explorer** で `ToDoActivity.java` ファイルを開き、次の import ステートメントを追加します。

    ```java
    import java.util.concurrent.ExecutionException;
    import java.util.concurrent.atomic.AtomicBoolean;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;

    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
    ```

3. **ToDoActivity** クラスに次のメソッドを追加します。

    ```java
    // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
    public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

    private void authenticate() {
        // Login using the Google provider.
        mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the login request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // login succeeded
                    createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    createTable();
                } else {
                    // login failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

    このコードで、Google 認証プロセスを処理するメソッドが作成されます。 ダイアログに認証されたユーザーの ID が表示されます。 認証に成功した場合のみ続行できます。

    > [!NOTE]
    > Google 以外の ID プロバイダーを使用している場合は、**login** メソッドに渡す値を、_MicrosoftAccount_、_Facebook_、_Twitter_、_windowsazureactivedirectory_ のいずれかに変更します。

4. **onCreate** メソッドで、`MobileServiceClient` オブジェクトをインスタンス化するコードの後に、次のコード行を追加します。

    ```java
    authenticate();
    ```

    この呼び出しで、認証プロセスが開始されます。

5. **onCreate** メソッド内の `authenticate();` の後の残りのコードを新しい **createTable** メソッドに移動します。

    ```java
    private void createTable() {

        // Get the table instance to use.
        mToDoTable = mClient.getTable(ToDoItem.class);

        mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

        // Create an adapter to bind the items with the view.
        mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
        ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
        listViewToDo.setAdapter(mAdapter);

        // Load the items from Azure.
        refreshItemsFromTable();
    }
    ```

6. リダイレクトが適切に機能するように、次に示す `RedirectUrlActivity` のスニペットを `AndroidManifest.xml` に追加します。

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}"
                android:host="easyauth.callback"/>
        </intent-filter>
    </activity>
    ```

7. `redirectUriScheme` を Android アプリケーションの `build.gradle` に追加します。

    ```gradle
    android {
        buildTypes {
            release {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
            debug {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
        }
    }
    ```

8. `com.android.support:customtabs:23.0.1` を `build.gradle` の依存関係に追加します。

    ```gradle
    dependencies {
        // ...
        compile 'com.android.support:customtabs:23.0.1'
    }
    ```

9. **[Run (実行)]** メニューの **[Run app (アプリの実行)]** をクリックしてアプリを開始し、選択した ID プロバイダーでサインインします。

> [!WARNING]
> 記載されている URL スキームは、大文字と小文字が区別されます。 `{url_scheme_of_you_app}` のすべての出現箇所で大文字と小文字を同じように使用してください。

サインインに成功すると、アプリはエラーなしで実行されます。また、バックエンド サービスにクエリを実行したり、データを更新したりできるようになります。
