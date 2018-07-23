# [Fathom Analytics](https://usefathom.com) for [Trellis](https://roots.io/trellis)

Fathom Analytics is simple, trustworthy, and privacy-focused —
none of Google's BS!

**This project is a work in progress. Feel free to chip in!**

## To do

For reference: [Installing & running Fathom](https://github.com/usefathom/fathom/wiki/Installing-&-running-Fathom)

- [x] Download and install Fathom
- [ ] Create directory for Fathom site and scaffold `.env`
- [ ] Create Fathom database
- [ ] Register Fathom users
- [ ] Set up NGINX server block
- [ ] Generate Let's Encrype certificates
- [ ] Create Fathom service + start it

## Instructions

_None of the following works right now, but ideally, this would
be the API for setting it all up._

When setting your `wordpress_sites.yml`, this creates an option
called: `fathom`.

```yaml
wordpress_sites:
  example.com:
    # ...
    fathom: 
      enabled: true
      subdomain: fathom
```

If you have SSL enabled, a Let's Encrypt certificate will
automatically be generated for Fathom.

In your `vault.yml`:

```yaml
vault_wordpress_sites:
  example.com:
    env:
    # ...
    fathom:
      - secret: "generateme"
    fathom_users:
      - email: user@example.com
        password: example_password
```

Once you have everything set, you need to reprovision your
server. Then in your theme or plugin you can load the tracker.

```php
/**
 * Load Fathom Analytics in the footer
 */
add_action('wp_footer', function() {
    $loadFathom = (!defined('WP_ENV') || \WP_ENV == 'production') && !current_user_can('manage_options');
    ?>
    <?php if ($loadFathom): ?>
    <script>
    (function(f, a, t, h, o, m){
        a[h]=a[h]||function(){
            (a[h].q=a[h].q||[]).push(arguments)
        };
        o=f.createElement('script'),
        m=f.getElementsByTagName('script')[0];
        o.async=1; o.src=t; o.id='fathom-script';
        m.parentNode.insertBefore(o,m)
    })(document, window, '//fathom.example.com/tracker.js', 'fathom');
    fathom('trackPageview');
    </script>
    <?php endif; ?>
<?php
}, 100);
```

Go to `fathom.example.com` and sign in with the account you
created to start seeing your analytics.
