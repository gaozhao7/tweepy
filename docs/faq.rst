.. _faq:

.. currentmodule:: tweepy

**************************
Frequently Asked Questions
**************************

General
=======

Why am I encountering a 401 Unauthorized error?
-----------------------------------------------

If you're using a method that performs an action on behalf of the
authenticating user, e.g. :meth:`API.update_status`, make sure your app has the
write permission.

After giving it the write permission, make sure to regenerate and use new
credentials to utilize it.

See `Twitter's API documentation on app permissions`_ for more information.

.. _Twitter's API documentation on app permissions: https://developer.twitter.com/en/docs/apps/app-permissions

Why am I encountering issues when attempting to upload GIFs or videos?
----------------------------------------------------------------------

If you are encountering a 400 Bad Request error when uploading large GIFs or
other errors/issues with uploading videos, make sure to pass the
``media_category`` parameter, e.g. as ``tweet_gif`` or ``tweet_video``.

Also make sure your video follows the recommended specifications.

See `Twitter's API documentation on media best practices`_ for more information.

.. _Twitter's API documentation on media best practices: https://developer.twitter.com/en/docs/twitter-api/v1/media/upload-media/uploading-media/media-best-practices

Why am I getting an inconsistent number of / not getting 3200 Tweets for a specific user?
-----------------------------------------------------------------------------------------

    "For very high volume and high traffic accounts, be aware that the Twitter
    API is highly distributed and eventually consistent. We strive to provide
    current information but like any large scale system, you may see unexpected
    behaviours at high volumes."

https://twittercommunity.com/t/inconsistent-tweet-retrieval/150635

Why am I getting no results from :meth:`API.search_tweets`?
-----------------------------------------------------------

Twitter's standard search API only "searches against a sampling of recent
Tweets published in the past 7 days."

If you're specifying an ID range beyond the past 7 days or there are no results
from the past 7 days, then no results will be returned.

See Twitter's documentation on the standard search API for more information:
https://developer.twitter.com/en/docs/twitter-api/v1/tweets/search/overview
https://developer.twitter.com/en/docs/twitter-api/v1/tweets/search/api-reference/get-search-tweets

Tweepy v4
=========

Why am I getting a :class:`TypeError` about an :class:`API` method taking 1 positional argument but given 2?
------------------------------------------------------------------------------------------------------------

This and other similar errors are due to :ref:`Tweepy v4.0.0 <Version 4.0.0>`
changing :class:`API` methods to no longer accept arbitrary positional
arguments. The 1 positional argument being referred to in the error is
``self``.

These parameters can be passed as keyword arguments instead.

Refer to the documentation for the :class:`API` method being used.

Where did ``API.me`` go?
------------------------

If you're attempting to use ``API.me`` with Tweepy v4, you'll get an
:class:`AttributeError` about the :class:`API` object not having a ``me``
attribute.

This is because :ref:`Tweepy v4.0.0 <Version 4.0.0>` removed ``API.me``.

Instead, you can use :meth:`API.verify_credentials`.

Where did ``StreamListener`` go?
--------------------------------

If you're attempting to import ``StreamListener`` with Tweepy v4, you'll get an
:class:`AttributeError` about ``tweepy`` not having a ``StreamListener``
attribute.

This is because :ref:`Tweepy v4.0.0 <Version 4.0.0>` merged ``StreamListener``
into :class:`Stream`.

To use Tweepy v4, you'll need to update your code to subclass :class:`Stream`
instead.

Twitter API v2
==============

Why am I not getting expansions or ``includes`` data with API v2 using :class:`Client`?
---------------------------------------------------------------------------------------

If you are simply printing the objects and looking at that output, the string
representations of API v2 models/objects only include the attributes that are
guaranteed to exist.

The objects themselves still include the relevant data, which you can access as
attributes or by key, like a dictionary.

There's also a ``data`` attribute/key that provides the entire data dictionary.

How do I access ``includes`` data while using :class:`Paginator`?
-----------------------------------------------------------------

:meth:`Paginator.flatten` flattens the ``data`` and iterates over each object.

To access ``includes``, you'll need to iterate through each response instead.

Why am I getting rate-limited so quickly when using :meth:`Client.search_all_tweets` with :class:`Paginator`?
-------------------------------------------------------------------------------------------------------------

The `GET /2/tweets/search/all`_ Twitter API endpoint that
:meth:`Client.search_all_tweets` uses has an additional 1 request per second
rate limit that is not handled by :class:`Paginator`.

You can :func:`time.sleep` 1 second while iterating through responses to handle
this rate limit.

.. _GET /2/tweets/search/all: https://developer.twitter.com/en/docs/twitter-api/tweets/search/api-reference/get-tweets-search-all
