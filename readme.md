Work in progress. It's functioning but missing tests. Use at your own peril.

# JsonReader

[![Build Status](https://travis-ci.org/pcrov/JsonReader.svg?branch=master)](https://travis-ci.org/pcrov/JsonReader)
<!--
[![License](https://poser.pugx.org/pcrov/jsonreader/license)](https://github.com/pcrov/JsonReader/blob/master/LICENSE)
[![Latest Stable Version](https://poser.pugx.org/pcrov/jsonreader/v/stable)](https://packagist.org/packages/pcrov/jsonreader)
[![Total Downloads](https://poser.pugx.org/pcrov/jsonreader/downloads)](https://packagist.org/packages/pcrov/jsonreader)
[![Latest Unstable Version](https://poser.pugx.org/pcrov/jsonreader/v/unstable)](https://packagist.org/packages/pcrov/jsonreader)
-->

This is a streaming pull parser - like [XMLReader](http://php.net/xmlreader), but for JSON.

Assumes UTF-8 encoded JSON, though does not strictly enforce it.

## Requirements

PHP 7+ and the Intl extension

## Installation

<!--
To install with composer:

```sh
composer require pcrov/jsonreader
```
-->

## Usage

JsonReader's api and behavior is very much like [XMLReader](http://php.net/xmlreader). If you've worked with that then
this will feel familiar.

### Class synopsis
```php
class JsonReader
{
    const NONE;
    const STRING;
    const NUMBER;
    const BOOL;
    const NULL;
    const ARRAY;
    const OBJECT;
    
    /**
     * Close the JsonReader input.
     *
     * @return void
     */
    public function close();

    /**
     * Depth of the node in the tree, starting at 0.
     *
     * @return int
     */
    public function getDepth() : int;

    /**
     * Name of the current node if any (for object properties).
     *
     * @return string|null
     */
    public function getName();

    /**
     * Type of the current node.
     *
     * @return int One of the JsonReader constants.
     */
    public function getNodeType() : int;

    /**
     * Value of the current node.
     *
     * For array and object nodes this will be evaluated on demand.
     *
     * Objects will be returned as arrays with strings for keys. Trying to return stdClass objects would gain nothing
     * but exposure to edge cases where valid JSON produces property names that are not allowed in PHP objects (e.g. ""
     * or "\u0000".) The behavior of `json_decode()` in these cases is inconsistent and can introduce key collisions, so
     * we'll not be following its lead.
     *
     * @return mixed
     */
    public function getValue();

    /**
     * Initializes the reader with the given parser.
     *
     * You do not need to call this if you're using one of the json() or open() methods.
     *
     * @param \Traversable $parser
     * @return void
     */
    public function init(\Traversable $parser);

    /**
     * Initializes the reader with the given JSON string.
     *
     * This convenience method handles creating the parser and relevant dependencies.
     *
     * @param string $json
     * @return void
     */
    public function json(string $json);

    /**
     * Move to the next node, skipping subtrees.
     *
     * If a name is given it will continue until a node of that name is reached.
     *
     * @param string|null $name
     * @return bool
     * @throws \pcrov\JsonReader\Exception
     */
    public function next(string $name = null) : bool;

    /**
     * Initializes the reader with the given file URI.
     *
     * This convenience method handles creating the parser and relevant dependencies.
     * 
     * @param string $uri
     * @return void
     */
    public function open(string $uri);

    /**
     * Move to the next node.
     * 
     * @return bool
     * @throws \pcrov\JsonReader\Exception
     */
    public function read() : bool;
}
```

\* Trying to return stdClass objects gains us nothing but exposure to edge cases where valid JSON produces property names
that are not allowed in PHP objects (e.g. "" or "\u0000".) The behavior of `json_decode()` in these cases is inconsistent
and can introduce key collisions, so we'll not be following its lead.

### Example
```php
use pcrov\JsonReader\JsonReader;
```
