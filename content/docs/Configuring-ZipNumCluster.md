+++
title = 'Configuring ZipNumCluster'
date = '2025-09-26T21:03:00+10:00'
draft = false
+++

See Aaron Swartz's post on [ZipNum and CDX cluster merging](http://aaron.blog.archive.org/2013/05/28/zipnum-and-cdx-cluster-merging/) for background.

Enable the `ZipNumClusterSearchResultSource` inside `CDXCollection.xml`:

```xml
<property name="resourceIndex">
  <bean class="org.archive.wayback.resourceindex.LocalResourceIndex">
    <property name="canonicalizer" ref="waybackCanonicalizer" />
    <property name="source">
      <bean class="org.archive.wayback.resourceindex.ZipNumClusterSearchResultSource">
        <property name="cluster">
          <bean class="org.archive.format.gzip.zipnum.ZipNumCluster">
            <property name="summaryFile" value="/<PATH-TO-SUMMARYFILE>" />
            <property name="locFile" value="/<PATH-TO-LOCFILE>" />
          </bean>
        </property>
        <property name="params">
          <bean class="org.archive.format.gzip.zipnum.ZipNumParams" />
        </property>
      </bean>
    </property>
    <property name="maxRecords" value="100000" />
    <property name="dedupeRecords" value="true" />
  </bean>
</property>
```

### Summary file format

Tab-separated columns:

1. First capture line in the chunk
2. Chunk (shard) name
3. Byte offset where the chunk starts
4. Chunk length in bytes

### Loc file format

Tab-separated columns:

1. Chunk (shard) name
2. Chunk URL: e.g. `hdfs://...` or `http://...`

Generate summary and loc files with the Hadoop tools linked in the article above.
