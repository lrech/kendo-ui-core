---
title: Overview
page_title: PanelBar | Telerik UI for ASP.NET MVC HtmlHelpers
description: "Get started with the server-side wrapper for the Kendo UI PanelBar widget for ASP.NET MVC."
slug: overview_panelbarhelper_aspnetmvc
position: 1
---

# PanelBar HtmlHelper Overview

The PanelBar HtmlHelper extension is a server-side wrapper for the [Kendo UI PanelBar](http://docs.telerik.com/kendo-ui/api/javascript/ui/panelbar) widget.

## Getting Started

### The Basics

There are a few ways to bind a Kendo UI PanelBar for ASP.NET MVC:

* Use items builder&mdash;Manually define the properties of each PanelBar item.
* Sitemap binding&mdash;Uses a sitemap to create the items of the PanelBar.
* Model binding&mdash;Uses a collection of objects to create the items of the Menu.

### Items Builder

Below are listed the steps for you to follow when defining the items of a Kendo UI PanelBar.

1. Make sure you followed all the steps from the [introductory article on Telerik UI for ASP.NET MVC]({% slug overview_aspnetmvc %}).

1. Create a new action method which renders the view.

    ###### Example

        public ActionResult Index()
        {
            return View();
        }

1. Add a PanelBar.

    ```ASPX
        <%: Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .Items(items =>
            {
                items.Add().Text("Item 1"); //Add item with text "Item1")
                items.Add().Text("Item 2"); //Add item with text "Item2")
            })
        %>
    ```
    ```Razor
        @(Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .Items(items =>
            {
                items.Add().Text("Item 1"); //Add item with text "Item1")
                items.Add().Text("Item 2"); //Add item with text "Item2")
            })
        )
    ```

### Expand Mode

The PanelBar provides the `Single` or `Multiple` expand mode options.

* If `ExpandMode` is set to `Single`, the user can expand only a single root item or a single child item of a specific parent item. Expanding another root item or another child of the currently expanded item's parent will collapse the currently expanded item. This approach is also the only way to collapse an expanded item in the `single` expand mode.
* If `ExpandMode` is set to `Multiple`, the user can expand multiple root items or children of the same parent item at a time. Expanding an item does not collapse the currently expanded items. Expanded items can be collapsed by clicking on them.

```Razor
    @(Html.Kendo().PanelBar()
        .Name("panelbar")
        .ExpandMode(PanelBarExpandMode.Single)
        .Items(items =>
        {
            items.Add().Text("Root1")
                .Items(subitems =>
                {
                    subitems.Add().Text("Level2 1");
                    subitems.Add().Text("Level2 2");
                });
            items.Add().Text("Root2")
                .Items(subitems =>
                {
                    subitems.Add().Text("Level2 1");
                    subitems.Add().Text("Level2 2");
                });
        })
    )
```
```ASPX
    <%: Html.Kendo().PanelBar()
        .Name("panelbar")
        .ExpandMode(PanelBarExpandMode.Single)
        .Items(items =>
        {
            items.Add().Text("Root1")
                .Items(subitems =>
                {
                    subitems.Add().Text("Level2 1");
                    subitems.Add().Text("Level2 2");
                });
            items.Add().Text("Root2")
                .Items(subitems =>
                {
                    subitems.Add().Text("Level2 1");
                    subitems.Add().Text("Level2 2");
                });
        })
    %>
```

### Sitemap Binding

Below are listed the steps for you to follow when binding a Kendo UI PanelBar to a sitemap.

1. Make sure you followed all the steps from the [introductory article on Telerik UI for ASP.NET MVC]({% slug overview_aspnetmvc %}).

1. Create a simple sitemap with a `sample.sitemap` file name at the root of the project.

    ###### Example

        <?xml version="1.0" encoding="utf-8" ?>
        <siteMap>
            <siteMapNode title="Home" controller="Home" action="Overview">
            <siteMapNode title="Grid">
                <siteMapNode controller="grid" action="index" title="First Look (Razor)" area="razor"/>
                <siteMapNode controller="grid" action="index" title="First Look (ASPX)" area="aspx"/>
            </siteMapNode>
            <siteMapNode title="PanelBar">
                <siteMapNode controller="panelbar" action="index" title="First Look (Razor)" area="razor"/>
                <siteMapNode controller="panelbar" action="index" title="First Look (ASPX)" area="aspx"/>
            </siteMapNode>
            </siteMapNode>
        </siteMap>

1. Load the sitemap using the `SiteMapManager`.

    ###### Example

        public ActionResult Index()
        {
            if (!SiteMapManager.SiteMaps.ContainsKey("sample"))
            {
                SiteMapManager.SiteMaps.Register<XmlSiteMap>("sample", sitmap => sitmap.LoadFrom("~/sample.sitemap"));
            }
            return View();
        }

1. Add a PanelBar.

    ```ASPX
        <%: Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .BindTo("sample") //bind to sitemap with name "sample"
        %>
    ```
    ```Razor
        @(Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .BindTo("sample") //bind to sitemap with name "sample"
        )
    ```

### Model Binding

Below are listed the steps for you to follow when binding a kendo UI PanelBar to a hierarchical model.

1. Make sure you followed all the steps from the [introductory article on Telerik UI for ASP.NET MVC]({% slug overview_aspnetmvc %}).

1. Create a new action method and pass the **Categories** table as the model. Note that the **Categories** should be associated to the **Products** table.

    ###### Example

        public ActionResult Index()
        {
            NorthwindDataContext northwind = new NorthwindDataContext();

            return View(northwind.Categories);
        }

1. Make your view strongly typed.

    ```ASPX
        <%@ Page Language="C#" MasterPageFile="~/Views/Shared/Site.Master"
            Inherits="System.Web.Mvc.ViewPage<IEnumerable<MvcApplication1.Models.Category>>" %>
    ```
    ```Razor
        @model IEnumerable<MvcApplication1.Models.Category>
    ```

1. Add a PanelBar.

    ```ASPX
        <%: Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .BindTo(Model, mappings =>
            {
                mappings.For<category>(binding => binding //define first level of panelbar
                    .ItemDataBound((item, category) => //define mapping between panelbar item properties and the model properties
                    {
                        item.Text = category.CategoryName;
                    })
                    .Children(category => category.Products)); //define which property of the model contains the children
                mappings.For<product>(binding => binding
                    .ItemDataBound((item, product) =>
                    {
                        item.Text = product.ProductName;
                    }));
            })
        %>
    ```
    ```Razor
        @(Html.Kendo().PanelBar()
            .Name("panelbar") //The name of the panelbar is mandatory. It specifies the "id" attribute of the widget.
            .BindTo(Model, mappings =>
            {
                mappings.For<category>(binding => binding //define first level of panelbar
                    .ItemDataBound((item, category) => //define mapping between panelbar item properties and the model properties
                        {
                        item.Text = category.CategoryName;
                        })
                    .Children(category => category.Products)); //define which property of the model contains the children
                mappings.For<product>(binding => binding
                    .ItemDataBound((item, product) =>
                        {
                        item.Text = product.ProductName;
                        }));
            })
        )
    ```

### Security Trimming

The Kendo UI PanelBar widget has a built-in security trimming functionality, which is enabled by default. If the URL, which the PanelBar item points to, is not authorized, then it is hidden. Security trimming depends on the [ASP.NET MVC Authorization](http://www.asp.net/mvc/tutorials/mvc-music-store/mvc-music-store-part-7). Every `action` method decorated with [`AuthorizeAttribute`](http://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute.aspx) checks whether the user is authorized and allows or forbids the request.

For more information on the ASP.NET MVC Authorization, refer to [this link](http://weblogs.asp.net/jgalloway/archive/2011/04/28/looking-at-how-asp-net-mvc-authorize-interacts-with-asp-net-forms-authorization.aspx).

The PanelBar hides the Menu item if the [`OnAuthorization`](http://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute.onauthorization.aspx) method returns
[`HttpUnauthorizedResult`](http://msdn.microsoft.com/en-us/library/system.web.mvc.httpunauthorizedresult.aspx).

To use a custom `AuthorizeAttribute`, refer to [this link](https://github.com/telerik/kendo-examples-asp-net-mvc/tree/master/kendo-menu-with-custom-authorization-attribute).

## Event Handling

You can subscribe to all PanelBar [events](http://docs.telerik.com/kendo-ui/api/javascript/ui/menu#events).

### By Handler Name

The following example demonstrates how to subscribe to events by a handler name.

```ASPX
    <%: Html.Kendo().PanelBar()
            .Name("panelbar")
            .Events(e => e
                .Expand("panelbar_expand")
                .Collapse("panelbar_collapse")
            )
    %>
    <script>
        function panelbar_collapse() {
            //Handle the collapse event
        }

        function panelbar_expand() {
            //Handle the expand event
        }
    </script>
```
```Razor
    @(Html.Kendo().PanelBar()
            .Name("panelbar")
            .Events(e => e
                .Expand("panelbar_expand")
                .Collapse("panelbar_collapse")
            )
    )
    <script>
        function panelbar_collapse() {
            //Handle the collapse event
        }

        function panelbar_expand() {
            //Handle the expand event
        }
    </script>
```

### By Template Delegate

The following example demonstrates how to subscribe to events by a template delegate.

###### Example

    @(Html.Kendo().PanelBar()
        .Name("panelbar")
        .Events(e => e
            .Expand(@<text>
                function() {
                    //Handle the expand event inline
                }
            </text>)
            .Collapse(@<text>
                function() {
                    //Handle the collapse event inline
                }
            </text>)
        )
    )

## Reference

### Existing Instances

To reference an existing Kendo UI PanelBar instance, use the [`jQuery.data()`](http://api.jquery.com/jQuery.data/) configuration option. Once a reference is established, use the [PanelBar API](http://docs.telerik.com/kendo-ui/api/javascript/ui/panelbar#methods) to control its behavior.

###### Example

    //Put this after your Kendo PanelBar for ASP.NET MVC declaration
    <script>
        $(function() {
            // Notice that the Name() of the panelbar is used to get its client-side instance
            var panelbar = $("#panelbar").data("kendoPanelBar");
        });
    </script>

## See Also

* [Telerik UI for ASP.NET MVC API Reference: PanelBarBuilder](http://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/PanelBarBuilder)
* [Overview of Telerik UI for ASP.NET MVC]({% slug overview_aspnetmvc %})
* [Fundamentals of Telerik UI for ASP.NET MVC]({% slug fundamentals_aspnetmvc %})
* [Scaffolding in Telerik UI for ASP.NET MVC]({% slug scaffolding_aspnetmvc %})
* [Overview of the Kendo UI PanelBar Widget](http://docs.telerik.com/kendo-ui/controls/navigation/panelbar/overview)
* [Telerik UI for ASP.NET MVC API Reference Folder](http://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc/AggregateFunction)
* [Telerik UI for ASP.NET MVC HtmlHelpers Folder]({% slug overview_barcodehelper_aspnetmvc %})
* [Tutorials on Telerik UI for ASP.NET MVC]({% slug overview_timeefficiencyapp_aspnetmvc6 %})
* [Telerik UI for ASP.NET MVC Troubleshooting]({% slug troubleshooting_aspnetmvc %})
