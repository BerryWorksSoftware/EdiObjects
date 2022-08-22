# EdiObjects (Coming Soon!)

(This is a previw of a new tool soon to be available.
There will be a free version with basic functionality,
and a premium version with additional features
available with commercial source code license.
For more information, contact scott@canabrook.org.
)

EdiObjects is a Java API that allows you to load an EDI interchange
into a Java object and access the EDI content
in your Java application.
All X12 transaction sets and EDIFACT message types are supported.

## First Simple Example

As a quick preview of the API, suppose we have an X12 interchange with one or more
purchase orders (transaction set 850),
and an EDIFACT interchange with one or more purchase orders (ORDERS message type).
The following Java snippet loads the X12 interchange, iterates over all the 850 transactions
within all the functional groups, and displays the purchase order number for each one.

```java
package com.berryworks.edireader.ediobjects.example;

import com.berryworks.edireader.ediobjects.EdiObjectsLoader;
import com.berryworks.edireader.ediobjects.model.Interchange;
import com.berryworks.edireader.ediobjects.model.Segment;
import com.berryworks.edireader.ediobjects.model.Transaction;
import com.berryworks.edireader.ediobjects.richsegments.x12.BEG;

import java.io.FileReader;

public class Example {
    public static void main(String[] args) {
        Interchange interchange;
        try {
            interchange = new EdiObjectsLoader().load(new FileReader("850-4010.edi"));
        } catch (Exception e) {
            throw new RuntimeException(e);
        }

        for (Transaction t : interchange.getTransactions()) {
            for (Segment segment : t.getSegments()) {
                switch (segment.getType()) {
                    case "BEG":
                        BEG beg = (BEG) segment;
                        System.out.printf("Purchase Order %s %n", beg.purchaseOrderNumber_03());
                }
            }
        }
    }
}
```



