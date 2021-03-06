---
title: Enviar notificações push para aplicações Android com Hubs de Notificação do Azure e Firebase Cloud Messaging | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar os Hubs de Notificação do Microsoft Azure e o Google Firebase Cloud Messaging para enviar notificações push para dispositivos Android.
services: notification-hubs
documentationcenter: android
keywords: notificações push, notificação push, notificação push para o android, fcm, firebase cloud messaging
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 0ec429407324210fca824ebd0734ddb39bce56ac
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256566"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Tutorial: Enviar notificações push para dispositivos Android através dos Hubs de Notificação do Azure e do Google Firebase Cloud Messaging
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

> [!IMPORTANT]
> Este artigo demonstra as notificações push com o Google Firebase Cloud Messaging (FCM). Se ainda utiliza o Google Cloud Messaging (GCM), veja [Enviar notificações push para dispositivos Android com Hubs de Notificação do Azure e GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).

Este tutorial mostra como utilizar os Hubs de Notificação do Azure e o Firebase Cloud Messaging para enviar notificações push para uma aplicação Android. Neste tutorial, irá criar uma aplicação Android em branco que recebe notificações push através do Firebase Cloud Messaging (FCM).

O código de conclusão para este tutorial pode ser transferido [aqui](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase) a partir do GitHub.

Neste tutorial, siga os passos seguintes:

> [!div class="checklist"]
> * Criar um projeto do Android Studio.
> * Criar um projeto do Firebase que suporte o Firebase Cloud Messaging.
> * Criar um hub de notificação.
> * Ligar a aplicação ao hub de notificação.
> * Testar a aplicação. 

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, tem de ter uma conta ativa do Azure. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/free/).

* Para além da conta ativa do Azure mencionada acima, este tutorial requer a versão mais recente do [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797).
* Android 2.3 ou superior para Firebase Cloud Messaging.
* É necessário o Repositório Google revisão 27 ou superior para o Firebase Cloud Messaging.
* Serviços do Google Play 9.0.2 ou superior para Firebase Cloud Messaging.
* A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais de Notification Hubs para aplicações com Android.

## <a name="create-an-android-studio-project"></a>Criar um projeto do Android Studio
1. No Android Studio, inicie um novo projeto de Android Studio.
   
    ![Android Studio - novo projeto](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Escolha o tamanho do **Telefone e tablet** e o **SDK mínimo** que pretende suportar. Clique depois em **Seguinte**.
   
    ![Android Studio – fluxo de trabalho da criação do projeto](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Escolha **Atividade Vazia** para a atividade principal, clique em **Seguinte** e, em seguida, clique em **Concluir**.

## <a name="create-a-firebase-project-that-supports-fcm"></a>Criar um projeto do Firebase que suporte o FCM
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Configurar um hub de notificação
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Configurar as definições do Firebase Cloud Messaging para o hub
1. Selecione **Google (GCM)** na categoria **DEFINIÇÕES DE NOTIFICAÇÃO**. 
2. Introduza a chave da API (chave do servidor do FCM) que copiou anteriormente da [consola Firebase](https://firebase.google.com/console/).
3. Selecione **Guardar** na barra de ferramentas.

    ![Notification Hubs do Azure – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

O hub de notificação está agora configurado para trabalhar com o Firebase Cloud Messaging e tem as cadeias de ligação para registar a sua aplicação para receber e enviar notificações push.

## <a id="connecting-app"></a>Ligar a aplicação ao hub de notificação
### <a name="add-google-play-services-to-the-project"></a>Adicionar serviços do Google Play para o projeto
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Adicionar bibliotecas dos Notification Hubs do Azure
1. No `Build.Gradle` ficheiro da **aplicação**, adicione as seguintes linhas na secção de **dependências**.
   
    ```java
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Adicione o repositório seguinte depois da secção de **dependências**.
   
    ```java
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }
    ```

### <a name="add-google-firebase-support"></a>Adicionar suporte do Google Firebase

1. No `Build.Gradle` ficheiro da **aplicação**, adicione as seguintes linhas na secção de **dependências**.
   
    ```java
        compile 'com.google.firebase:firebase-core:12.0.0'
    ```

2. Adicione o seguinte plug-in ao final do ficheiro. 
   
    ```java
    apply plugin: 'com.google.gms.google-services'
    ```

### <a name="updating-the-androidmanifestxml"></a>Atualizar o AndroidManifest.xml.
1. Para suportar o FCM, temos de implementar um serviço de escuta de ID da Instância no código que é utilizado para [obter os tokens de registo](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) com a [API FirebaseInstanceId da Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). Neste tutorial, o nome da classe é `MyInstanceIDService`. 
   
    Adicionar a seguinte definição de serviço ao ficheiro AndroidManifest.xml, no interior da etiqueta `<application>`. 
   
    ```xml
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
    ```

2. Depois de receber o nosso token de registo do FCM a partir da API FirebaseInstanceId, utilize-o para [efetuar o registo no Hub de Notificação do Azure](notification-hubs-push-notification-registration-management.md). Suportará este registo em segundo plano com um `IntentService` com o nome `RegistrationIntentService`. Este serviço também é responsável por atualizar o token de registo do FCM.
   
    Adicionar a seguinte definição de serviço ao ficheiro AndroidManifest.xml, no interior da etiqueta `<application>`. 
   
    ```xml
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
    ```

3. Também necessitará definir um recetor para receber notificações. Adicione a seguinte definição do recetor ao ficheiro AndroidManifest.xml, no interior da etiqueta `<application>`. Substitua o marcador de posição `<your package>` pelo nome do seu pacote real indicado no topo do ficheiro `AndroidManifest.xml`.

    ```xml
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
    ```

4. Adicione as seguintes permissões do FCM por baixo da etiqueta `</application>`. 
   
    Para obter mais informações sobre estas permissões, veja [Setup a GCM Client app for Android (Configurar uma aplicação de Cliente GCM para Android)](https://developers.google.com/cloud-messaging/android/client#manifest) e [Migrate a GCM Client App for Android to Firebase Cloud Messaging (Migrar uma aplicação de Cliente GCM para Android para o Firebase Cloud Messaging)](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
    ```xml
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Adicionar código
1. Na vista de projeto, expanda **app** > **src** > **principal** > **java**. Com o botão direito do rato, clique na pasta do pacote por baixo de **java**, clique em **Nova**, e, em seguida, clique em **Classe Java**. Adicione uma nova classe com o nome `NotificationSettings`. 
   
    ![Android Studio - nova classe Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Certifique-se de que atualiza esses três marcadores de posição no seguinte código para a classe `NotificationSettings`:
   
   * **SenderId**: o ID do remetente obtido anteriormente no separador **Cloud Messaging** das suas definições do projeto na [consola Firebase](https://firebase.google.com/console/).
   * **HubListenConnectionString**: A cadeia de ligação **DefaultListenAccessSignature** do seu hub. Pode copiar essa cadeia de ligação ao clicar em **Políticas de Acesso** no hub do [portal do Azure].
   * **HubName**: utilize o nome do hub de notificação que aparece na página do hub no [portal do Azure].
     
     Código `NotificationSettings`:
     
    ```java
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
    ```

2. Utilizando os passos anteriores, adicione outra nova classe com o nome `MyInstanceIDService`. Esta classe é a implementação do serviço de escuta de ID da Instância.
   
    O código para esta classe chama o `IntentService` para [atualizar o token do FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) em segundo plano.
   
    ```java
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };
    ```

1. Adicione outra nova classe ao projeto com o nome `RegistrationIntentService`. Esta classe implementa a interface `IntentService` e processa a [atualização do token do FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) e o [registo no hub de notificação](notification-hubs-push-notification-registration-management.md).
   
    Utilize o seguinte código para esta classe.
   
    ```java
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing the registration ID that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
    ```

2. Na sua classe `MainActivity`, adicione as seguintes `import` declarações acima da declaração de classe.
   
    ```java
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
    ```
3. Adicione os seguintes membros privados no topo da classe. Utiliza estes campos para [verificar a disponibilidade dos Serviços do Google Play, tal como recomendado pela Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
    ```java
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

4. Na sua classe `MainActivity`, adicione o seguinte método para a disponibilidade dos serviços do Google Play. 
   
    ```java
        /**
        * Check the device to make sure it has the Google Play Services APK. If
        * it doesn't, display a dialog that allows users to download the APK from
        * the Google Play Store or enable it in the device's system settings.
        */
    
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
    ```

5. Na sua classe `MainActivity`, adicione o seguinte código que verifica os Serviços do Google Play antes de chamar o `IntentService` para obter o token de registo do FCM e efetuar o registo no hub de notificação.
   
    ```java
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
    ```

6. No `OnCreate` método da `MainActivity` classe, adicione o seguinte código para iniciar o processo de registo quando é criada a atividade.
   
    ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
    ```

7. Para verificar o estado da aplicação e do relatório na aplicação, adicione estes métodos adicionais a `MainActivity`.
   
    ```java
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
    ```

8. O `ToastNotify` método utiliza o comando *"Olá, mundo"* `TextView` para comunicar o estado e as notificações de forma permanente na aplicação. No seu ctivity_main.xml layout, adicione a seguinte ID para esse comando.
   
    ```java
       android:id="@+id/text_hello"
    ```

9. Em seguida, adicionará uma subclasse do recetor que definiu no AndroidManifest.xml. Adicione outra nova classe ao projeto com o nome `MyHandler`.
10. Adicione as seguintes declarações de importação no topo do ficheiro `MyHandler.java`:
    
    ```java
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

11. Adicione o seguinte código à classe `MyHandler`, tornando-a numa subclasse de `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Este código substitui o método `OnReceive` para que o processador comunique as notificações que são recebidas. O processador também envia a notificação push para o gestor de notificações Android através do método `sendNotification()`. O método `sendNotification()` deve ser executado quando a aplicação não está em execução e deverá receber uma notificação.
    
    ```java
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
    ```

12. No Android Studio, na barra de menus, clique em **Construir** > **Reconstruir projeto** para se certificar de que não existem erros no seu código.
13. Execute a aplicação no seu dispositivo e verifique se a mesma é registada com êxito no hub de notificação. 
    
    > [!NOTE]
    > O registo pode falhar no lançamento inicial até que o método `onTokenRefresh()` do serviço do ID da instância seja chamado. A atualização deve iniciar um registo com êxito no hub de notificação.
    > 
    > 

## <a name="test-the-app"></a>Testar a aplicação
### <a name="test-send-notification-from-the-notification-hub"></a>Notificação de envio de teste a partir do hub de notificação
Pode enviar notificações push a partir do [portal do Azure] ao efetuar as seguintes ações: 

1. Selecione **Testar Envio** na secção **Resolução de Problemas**.
2. Para **Plataformas**, selecione **Android**. 
3. Selecione **Enviar**.  Ainda não verá uma notificação no dispositivo Android porque não executou a aplicação móvel no mesmo. Depois de executar a aplicação móvel, selecione novamente o botão **Enviar** para ver a mensagem de notificação. 
4. Veja o **resultado** da operação na lista na parte inferior. 

    ![Notification Hubs do Azure – Teste de Envio](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)


[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


### <a name="run-the-mobile-app"></a>Executar a aplicação móvel
Se pretender testar as notificações push no interior de um emulador, certifique-se de que a imagem do emulador suporta o nível da API da Google que escolheu para a sua aplicação. Se a imagem não suportar as APIs nativas da Google, termina com a exceção **SERVIÇO\_NÃO\_DISPONÍVEL**.

Além disso, confirme que adicionou a sua conta Google ao emulador em execução em **Definições** > **Contas**. Caso contrário, as tentativas para registar o GCM podem originar a exceção **FALHA DE\_AUTENTICAÇÃO**.

1. Execute a aplicação e repare que a ID de registo é comunicada para um registo de sucesso.
   
    ![Teste no Android - registo de canal](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Introduza uma mensagem de notificação que será enviada a todos os dispositivos Android registados no hub.
   
    ![Teste no Android - enviar uma mensagem](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Prima **Enviar notificação**. Todos os dispositivos que estejam a executar a aplicação mostram uma `AlertDialog` instância com a mensagem de notificação push. Dispositivos que não estejam a executar a aplicação, mas foram anteriormente registados para as notificações push, recebem uma notificação no gestor de notificações do Android. As notificações podem ser vistas ao percorrer para baixo a partir do canto superior esquerdo.
   
    ![Teste no Android - notificações](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, utilizou o Firebase Cloud Messaging para enviar notificações push para dispositivos Android. Para saber como enviar notificações push com o Google Cloud Messaging, avance para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Enviar notificações push para dispositivos Android com o Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md)


<!-- Images. -->


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Portal do Azure]: https://portal.azure.com
