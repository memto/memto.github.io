---
layout: post
ads: true
comments: true
published: true
title: mongoosastic(4.4.1) and elasticsearch(6.2.3)
categories:
  - linux
  - program
  - tooltips
tags:
  - elasticsearch
  - mongoosastic
---
#### Ref
	https://github.com/mongoosastic/mongoosastic
    https://www.elastic.co/blog/strings-are-dead-long-live-strings
    
#### How it works?
    const mongoose = require('mongoose');
    const mongoosastic = require('mongoosastic');

    const { Schema } = mongoose;

    const ProductSchema = new Schema({
      category: { type: Schema.Types.ObjectId, ref: 'Category' },
      name: String,
      price: Number,
      image: String,
    });

    ProductSchema.plugin(mongoosastic, {
      hosts: ['localhost:9200'],
    });

    module.exports = mongoose.model('Product', ProductSchema);
    
By using mongoosastic as plugin there shall be some function added to Product model (ex: Product.createMapping, Product.synchronize)

**createMapping** shall create a query with content map from mongo schema fields (ex: category, name) into elasticsarch fields

Ex: body: '{"product":{"properties":{"category":{"type":"string"},"name":{"type":"string"},"price":{"type":"long"},"image":{"type":"string"}}}

#### Issue
	'[mapper_parsing_exception] No handler for type [string] declared on field [category]',
    
This is because on version 6.x.x of elasticsearch _string_ is removed and replaced by _text_ type

Fix: add _customProperties_ when set _mognosastic_ plugin

    ProductSchema.plugin(mongoosastic, {
      hosts: ['localhost:9200'],
      customProperties: {
        category: {
          type: 'text',
        },
        name: {
          type: 'text',
        },
        price: {
          type: 'long',
        },
        image: {
          type: 'text',
        },
      },
    });
