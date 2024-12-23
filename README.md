> [!NOTE]
> This article is based on information available as of January 2021. Please check the official website for the latest updates.

Google Tag Manager's server-side tagging was released in 2020. Initially, data was sent from the browser to the Google Analytics server via a server. Later, a new "Web Container" was added to the server-side client.

This allows the container (gtm.js) to be issued from your own domain (1st Party), rather than from the traditional domain `www.googletagmanager.com`.

## Creating a Web Container

First, create a new "Web Container." Open the GTM server-side container, go to "Clients," and click "New."

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/09018859-feca-466b-ec27-9b62114acb57.png)

Name it "Web Container" and select "Google Tag Manager: Web Container" as the client type.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/8ee4d9c8-5b2a-dd2c-46d9-38ecc0bfaeec.png)

Scroll down to the "Serve gtm.js for specific IDs" section and add the container ID. The "container ID" refers to the GTM-XXXXXX string already added to your website.

Click the "Add Container ID" button, enter the registered ID (e.g., GTM-ABCDEF), and save it.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/01da8efd-3c38-ee81-2771-3aa7a6ae2fb0.png)

## Testing Tag Deployment

Once you've confirmed that the "Web Container" has been added, test its functionality. Click the "Preview" button.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/d2d1783e-772b-f60d-214b-e909bc8c845b.png)

If a debug screen like the one below appears, you're good to go.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/c828bf92-4025-2572-9a2d-115ffb88a5f6.png)

Next, open a new browser tab or window and enter `https://server-side.pep4.net/gtm.js?id=GTM-ABCDEF` (replace the domain and GTM container with your own). If the script below appears, the web container setup is successful.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/5380da69-fc78-d17b-4a83-1fe3620d65d5.png)

## Configuring the GA4 Client

Next, configure the GA4 client tag. If you've previously created a server-side container, a GA4 client should already exist. Modify it as needed.

In GTM, go to "Clients," select "GA4" (or the GA4 client you created earlier).

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/54430c50-a540-c377-551c-78f48385f0fe.png)

- Default GA4 path: Check
- Default gtag.js path for specific IDs: Check
- Measurement ID: Enter your GA4 Measurement ID (a string starting with "G-").

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/2b542590-ad74-688b-e4d6-ef893d3979c6.png)

Apply similar changes to the Universal Analytics client, if necessary.

## Updating GTM Code Snippets

Finally, modify the GTM tags embedded in your website. The code currently looks something like this:

In line 5:

`https://<span style="color: red">www.googletagmanager.com</span>/gtm.js?id=`

Replace it with the domain you created for the server-side tag (the value of the "transport_url" field in the GA4 configuration tag).

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3939399/6b7c79ad-c25f-9ffb-0212-2232fa760a8d.png)

**Before:**
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-PZ7GMV9');</script>
<!-- End Google Tag Manager -->
```

**After:**
```
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://server-side.pep4.net/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-PZ7GMV9');</script>
<!-- End Google Tag Manager -->
```

That's it! Publish the changes, and if page views are tracked in GA4's real-time reports, the setup is successful.
