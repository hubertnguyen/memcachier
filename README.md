# memcachier
Wordpress object-cache.php drop-in for Memcachier over plain Memcache protocol

This object-cache.php file can will make the original Memcached drop-in by Ryan Boren, Denis de Bernardy and Matt Martz work with Memcachier.com's Memcache-as-a-service product.

Memcachier has built a clever authentification system which increases the Memcache instance's data safety, without requiring the more recent ALSL authentification protocol. This was handy for me since I had some system which were still running old PHP libraries.

in object-cache.php, yhe modification is trivial. Right after the initial connection ->addserver(), the PHP code needs to send the credential with 

$this->mc[$bucket]->set(MEMCACHIER_USER, MEMCACHIER_PASSWORD);

This assumed that wp-config.php has been edited to add your credentials obtained from Memcachier's dashboard. 

in wp-config.php, it would look like this:

global $memcached_servers;
$memcached_servers = array('default' => array('aaaa.bbbb.cccc.datacenter.memcachier.com::11211'));
define('MEMCACHIER_USER', 'xxxxxx');
define('MEMCACHIER_PASSWORD', 'yyyyy');

Make sure that you add these lines *ABOVE* these lines

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');

