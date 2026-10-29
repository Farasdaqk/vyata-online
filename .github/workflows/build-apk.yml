package id.co.secaba.vyataon;

import android.app.Activity;
import android.content.ActivityNotFoundException;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.webkit.JavascriptInterface;
import android.webkit.ValueCallback;
import android.webkit.WebChromeClient;
import android.webkit.WebResourceRequest;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Toast;

import androidx.annotation.Nullable;
import androidx.core.content.FileProvider;
import androidx.webkit.WebViewAssetLoader;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

public class MainActivity extends Activity {
    private static final int REQ_FILE_CHOOSER = 4107;
    private WebView webView;
    private ValueCallback<Uri[]> pendingFileCallback;
    private Uri cameraOutputUri;
    private File cameraOutputFile;
    private volatile String chooserMode = "gallery";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().setStatusBarColor(android.graphics.Color.rgb(8, 17, 10));
        getWindow().setNavigationBarColor(android.graphics.Color.rgb(8, 17, 10));

        webView = new WebView(this);
        setContentView(webView);

        WebSettings settings = webView.getSettings();
        settings.setJavaScriptEnabled(true);
        settings.setDomStorageEnabled(true);
        settings.setDatabaseEnabled(true);
        settings.setAllowFileAccess(false);
        settings.setAllowContentAccess(true);
        settings.setJavaScriptCanOpenWindowsAutomatically(false);
        settings.setMediaPlaybackRequiresUserGesture(true);
        settings.setSupportMultipleWindows(false);

        WebViewAssetLoader assetLoader = new WebViewAssetLoader.Builder()
                .addPathHandler("/assets/", new WebViewAssetLoader.AssetsPathHandler(this))
                .build();

        webView.addJavascriptInterface(new NativeBridge(), "VYATAAndroid");
        webView.setWebViewClient(new WebViewClient() {
            @Override
            public android.webkit.WebResourceResponse shouldInterceptRequest(WebView view, WebResourceRequest request) {
                return assetLoader.shouldInterceptRequest(request.getUrl());
            }

            @Override
            @SuppressWarnings("deprecation")
            public android.webkit.WebResourceResponse shouldInterceptRequest(WebView view, String url) {
                return assetLoader.shouldInterceptRequest(Uri.parse(url));
            }

            @Override
            public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
                Uri uri = request.getUrl();
                if ("appassets.androidplatform.net".equals(uri.getHost())) return false;
                if ("https".equalsIgnoreCase(uri.getScheme()) || "http".equalsIgnoreCase(uri.getScheme())) {
                    try { startActivity(new Intent(Intent.ACTION_VIEW, uri)); }
                    catch (ActivityNotFoundException ignored) { }
                    return true;
                }
                return true;
            }

            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                // Tell native Android which file-input button was tapped, so camera and gallery stay distinct.
                String js = "(function(){document.querySelectorAll('input[type=file]').forEach(function(el){if(el.dataset.vyataNativeBound)return;el.dataset.vyataNativeBound='1';el.addEventListener('click',function(){try{if(window.VYATAAndroid){window.VYATAAndroid.setFileChooserMode(el.id==='lap-camera-input'?'camera':'gallery');}}catch(e){}},true);});})();";
                view.evaluateJavascript(js, null);
            }
        });

        webView.setWebChromeClient(new WebChromeClient() {
            @Override
            public boolean onShowFileChooser(WebView view, ValueCallback<Uri[]> filePathCallback, FileChooserParams fileChooserParams) {
                if (pendingFileCallback != null) pendingFileCallback.onReceiveValue(null);
                pendingFileCallback = filePathCallback;
                if ("camera".equals(chooserMode)) {
                    launchCamera();
                } else {
                    launchFilePicker(fileChooserParams);
                }
                return true;
            }
        });

        webView.loadUrl("https://appassets.androidplatform.net/assets/index.html");
    }

    private class NativeBridge {
        @JavascriptInterface
        public void setFileChooserMode(String mode) {
            if ("camera".equals(mode)) chooserMode = "camera";
            else chooserMode = "gallery";
        }
    }

    private void launchFilePicker(WebChromeClient.FileChooserParams params) {
        try {
            Intent intent = params.createIntent();
            startActivityForResult(intent, REQ_FILE_CHOOSER);
        } catch (Exception e) {
            Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
            intent.addCategory(Intent.CATEGORY_OPENABLE);
            intent.setType("*/*");
            try { startActivityForResult(Intent.createChooser(intent, "Pilih berkas"), REQ_FILE_CHOOSER); }
            catch (ActivityNotFoundException ex) { finishFileChooser(null); }
        }
    }

    private void launchCamera() {
        Intent camera = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
        if (camera.resolveActivity(getPackageManager()) == null) {
            Toast.makeText(this, "Aplikasi kamera tidak ditemukan. Silakan pilih gambar dari galeri.", Toast.LENGTH_LONG).show();
            launchGenericImagePicker();
            return;
        }
        try {
            cameraOutputFile = createCameraFile();
            cameraOutputUri = FileProvider.getUriForFile(this, getPackageName() + ".fileprovider", cameraOutputFile);
            camera.putExtra(android.provider.MediaStore.EXTRA_OUTPUT, cameraOutputUri);
            camera.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION | Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
            startActivityForResult(camera, REQ_FILE_CHOOSER);
        } catch (Exception e) {
            Toast.makeText(this, "Kamera tidak dapat dibuka. Coba pilih gambar dari galeri.", Toast.LENGTH_LONG).show();
            launchGenericImagePicker();
        }
    }

    private File createCameraFile() throws IOException {
        String stamp = new SimpleDateFormat("yyyyMMdd_HHmmss", Locale.US).format(new Date());
        return File.createTempFile("VYATA_" + stamp + "_", ".jpg", getCacheDir());
    }

    private void launchGenericImagePicker() {
        try {
            Intent pick = new Intent(Intent.ACTION_GET_CONTENT);
            pick.addCategory(Intent.CATEGORY_OPENABLE);
            pick.setType("image/*");
            startActivityForResult(Intent.createChooser(pick, "Pilih gambar"), REQ_FILE_CHOOSER);
        } catch (ActivityNotFoundException e) {
            finishFileChooser(null);
        }
    }

    @Override
    @SuppressWarnings("deprecation")
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode != REQ_FILE_CHOOSER) return;
        Uri[] results = null;
        if (resultCode == RESULT_OK) {
            if (cameraOutputUri != null && "camera".equals(chooserMode)) {
                if (cameraOutputFile != null && cameraOutputFile.exists() && cameraOutputFile.length() > 0) {
                    results = new Uri[]{cameraOutputUri};
                } else if (data != null && data.getData() != null) {
                    results = new Uri[]{data.getData()};
                }
            } else if (data != null) {
                Uri[] parsed = WebChromeClient.FileChooserParams.parseResult(resultCode, data);
                if (parsed != null && parsed.length > 0) results = parsed;
                else if (data.getData() != null) results = new Uri[]{data.getData()};
            }
        }
        finishFileChooser(results);
        cameraOutputUri = null;
        cameraOutputFile = null;
    }

    private void finishFileChooser(Uri[] result) {
        if (pendingFileCallback != null) {
            pendingFileCallback.onReceiveValue(result);
            pendingFileCallback = null;
        }
    }

    @Override
    public void onBackPressed() {
        if (webView != null && webView.canGoBack()) webView.goBack();
        else super.onBackPressed();
    }

    @Override
    protected void onDestroy() {
        if (webView != null) {
            webView.destroy();
            webView = null;
        }
        super.onDestroy();
    }
}
