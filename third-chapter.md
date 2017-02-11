# 第三章 函数

从早期使用Fortran开始，通过程序和子程序构成一个系统，到现在为止，program包含的特性只有Function这一项留到了现在，下面是摘自FitNesse工具的一段代码，这个函数不仅很长而且还有重复的code、奇怪的字符串，很多奇怪的数据类型和APIs。尝试着读读看，看你能理解多少。

    public static SeSetup) throws Exception {
        WikiPage wikiPage = pageData.getWikiPage();
        StringBuffer buffer = new StringBuffer();
        if (pageData.hasAttribute("Test")) {
            if (includeSuiteSetup) {
                WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
                if (suiteSetup! = null) {
                    WikiPagePath pagePath = suiteSetup.getPageCrawler().getFullPath(suiteSetup);
                    String pagePathName = PathParser.render(pagePath);
                    buffer.append("!include -setup.").
                        .append(pagePathName)
                        .append("\n");
                }
            }
        
            WikiPage setup = PageCrawlerImpl.getInteritedPage("Setup", wikiPage);
            if (setup != null) {
                WikiPagePath setupPath = wikiPage.getPageCrawler().getFullPath(setup);
                String setupPathName = PathParser.render(setupPath);
                buffer.append("!include -setup.")
                    .append(setupPathName)
                    .append("\n");
            }
        }
        
        buffer.append(pageData.getContent);
        
        if (pageData.hasAttribute("Test")) {
            WikiPage teardown = PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
            if (teardown != null) {
                WikiPagePath tearDownPath = wikiPage.getPageCrawler().getFullPath(teardown);
                String tearDownPathName = PathPaser.rander(tearDownPath);
                buffer.append("!include -teardown.")
                    .append(tearDownPathName)
                    .append("\n");
            }
            
            if (includeSuiteSetup) {
                WikiPage pagePath = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
                if (suiteTearDown != null) {
                    WikiPagePath pagePath = suiteTearDown.getPageCrawler().getFullPath(suiteTeardown);
                String pagePathName = PathPaser.rander(pagePath);
                buffer.append("!include -teardown.")
                    .append(pagePathName)
                    .append("\n");

                }
            }
        }
        pageData.setContent(buffer.toString());
        return pageData.getHtml();
    }
    
如果给你三分钟阅读时间，你能够弄明白这个函数吗？很有可能是不行的，这个代码显然不那么平缓，而且还有很多级抽象，以及大量字符串和调用，然而我们也有办法，通过一些命名和重构来解决这个问题。看下面的代码：

    public static String renderPageWithSetupsAndTeardowns (PageData pageData, boolean isSuite) throws Exception {
        boolean isTestPage = pageData.getAttribute.hasAttribute("Test");
        if (isTestPage) {
            WikiPage testPage = pageData.getWIkiPage();
            StringBuffer new PageConent = new StringBuffer();
            includeSetupPages(testPage, newPageContent, isSuite);
            newPageContent.append(pageData.getContent());
            includeTeardownPages(testPage, newPageContent, isSuite);
            pageData.setContent(newPageContent.toString());
        }
        
        return pageData.getHtml();
    }

除非你是学习FitNesse代码的学生，否则你不能直接看出代码的很多细节，不过现在理解这个代码的行为对你来说是比较容易的。如果你熟悉JUnit的话就很容易看出这是属于某个测试框架的代码。
