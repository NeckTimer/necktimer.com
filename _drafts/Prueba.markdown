---
layout: default
title: Prueba
permalink: /prueba
---

<h1 style="color: gray"><b>Recent Articles</b></h1>

- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
[ ] \(Optional) Abre una propuesta de seguimiento


```swift
  import Foundation
  import Combine

  final class OrderData: ObservableObject {
      
      @Published var vOrder: [Course] = []
      
      func añadePlato(plato: Course) -> Void {
          vOrder.append(plato)
      }

      func sumaPedido(todo: [Course]) -> Double {
          var suma: Double = 0
          for pedido in todo {
              suma = suma + pedido.price
          }
          return suma
      }
      
      func borraPlato(at offsets: IndexSet){
          vOrder.remove(atOffsets: offsets)
      }
  }
```


<table style="width: 100%; horizontal-align: center">
  <tr>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <a style="color: #403F3F" href="https://www.javiercarrilloblog.com/coding/15/04/2021/Formula1WebScraping.html"><h2><b>Open-Sourcing Pulse</b></h2></a>
      <p style="color: gray">Open source developers</p>
    </td>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <h2><b>LazyImage</b></h2>
      <p style="color: gray">LazyImage loading for Swift</p>
    </td>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <h2><b>Client-Search Side</b></h2>
      <p style="color: gray">How to build client-side applications</p>
    </td>
  </tr>
  <tr style="background-color: #FDFDFD">
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <h2><b>CryptoKit</b></h2>
      <p style="color: gray">Basics about CryptoKit for Swift</p>
    </td>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <h2><b>Pyhton `roject</b></h2>
      <p style="color: gray">My last python projects</p>
    </td>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: top">
      <h2><b>WebScrapping with Ruby</b></h2>
      <p style="color: gray">Parse the web with this app</p>
    </td>
  </tr>
</table>

<table style="width: 100%; horizontal-align: center; margin-left: auto; margin-right: auto">
  <tr>
    <td style="border-style: hidden; width: 50%; text-align: left; vertical-align: center">
      <header style="background-color: #F4F4F4; border-radius: 10px; padding: 20px">
        <a style="color: #403F3F" href="https://www.javiercarrilloblog.com/coding/15/04/2021/Formula1WebScraping.html">
        <table>
          <tr>
            <h2><b>Link</b></h2>
          </tr>
          <tr>
            <p>Esto es una prueba</p>
          </tr>
          <tr>
            <p>🟠Swift   ★146</p>
          </tr>
        </table>
        </a>
      </header>
    </td>
    <td style="border-style: hidden; width: 50%; text-align: left; vertical-align: center">
      <header style="background-color: #F4F4F4; border-radius: 10px; padding: 20px">
        <table>
          <tr>
            <h2><b>PruebaS</b></h2>
          </tr>
          <tr>
            <p>Esto es una prueba</p>
          </tr>
          <tr>
            <p>🟠Swift   ★146</p>
          </tr>
        </table>
      </header>
    </td>
  </tr>
  <tr style="background-color: #FDFDFD">
    <td style="border-style: hidden; width: 50%; text-align: left; vertical-align: center">
      <header style="background-color: #F4F4F4; border-radius: 10px; padding: 20px">
        <table>
          <tr>
            <h2><b>Prueba</b></h2>
          </tr>
          <tr>
            <p>Esto es una prueba</p>
          </tr>
          <tr>
            <p>🟠Swift   ★146</p>
          </tr>
        </table>
      </header>
    </td>
    <td style="border-style: hidden; width: 33%; text-align: left; vertical-align: center">
      <header style="background-color: #F4F4F4; border-radius: 10px; padding: 20px">
        <table>
          <tr>
            <h2><b>Prueba</b></h2>
          </tr>
          <tr>
            <p>Esto es una prueba</p>
          </tr>
          <tr>
            <p>🟠Swift   ★146</p>
          </tr>
        </table>
      </header>
    </td>
  </tr>
</table>