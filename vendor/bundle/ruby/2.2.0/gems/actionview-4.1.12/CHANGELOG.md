## Rails 4.1.12 (June 25, 2015) ##

*   `translate` should handle `raise` flag correctly in case of both main and default
    translation is missing.

    Fixes #19967

    *Bernard Potocki*

*   `translate` should accept nils as members of the `:default`
    parameter without raising a translation missing error.  Fixes a
    regression introduced 362557e.

    Fixes #19419

    *Justin Coyne*

*   `number_to_percentage` does not crash with `Float::NAN` or `Float::INFINITY`
    as input when `precision: 0` is used.

    Fixes #19227.

    *Yves Senn*


## Rails 4.1.11 (June 16, 2015) ##

*   No changes.


## Rails 4.1.10 (March 19, 2015) ##

*   Local variable in a partial is now available even if a falsy value is
    passed to `:object` when rendering a partial.

    Fixes #17373.

    *Agis Anastasopoulos*

*   Default translations that have a lower precidence than an html safe default,
    but are not themselves safe, should not be marked as html_safe.

    *Justin Coyne*

## Rails 4.1.9 (January 6, 2015) ##

*   Added an explicit error message, in `ActionView::PartialRenderer`
    for partial `rendering`, when the value of option `as` has invalid characters.

    *Angelo Capilleri*


## Rails 4.1.8 (November 16, 2014) ##

*   Update `select_tag` to work correctly with `:include_blank` option passing a string.

    Fixes #16483.

    *Frank Groeneveld*


## Rails 4.1.7.1 (November 19, 2014) ##

*   No changes.


## Rails 4.1.7 (October 29, 2014) ##

*   No changes.


## Rails 4.1.6 (September 11, 2014) ##

*   Fix that render layout: 'messages/layout' should also be added to the dependency tracker tree.

    *DHH*

*   Return an absolute instead of relative path from an asset url in the case
    of the `asset_host` proc returning nil

    *Jolyon Pawlyn*

*   Fix `html_escape_once` to properly handle hex escape sequences (e.g. &#x1a2b;)

    *John F. Douthat*

*   Bring `cache_digest` rake tasks up-to-date with the latest API changes

    *Jiri Pospisil*


## Rails 4.1.5 (August 18, 2014) ##

*   No changes.


## Rails 4.1.4 (July 2, 2014) ##

*   No changes.


## Rails 4.1.3 (July 2, 2014) ##

*   No changes.


## Rails 4.1.2 (June 26, 2014) ##

*   Change `asset_path` to use File.join to create proper paths.

        https://some.host.com//assets/some.js

    becomes

        https://some.host.com/assets/some.js

    *Peter Schröder*

*   `collection_check_boxes` respects `:index` option for the hidden field name.

    Fixes #14147.

    *Vasiliy Ermolovich*

*   `date_select` helper with option `with_css_classes: true` does not overwrite other classes.

    *Izumi Wong-Horiuchi*


## Rails 4.1.1 (May 6, 2014) ##

*   No changes.


## Rails 4.1.0 (April 8, 2014) ##

*   Fixed ActionView::Digestor template lookup to use the lookup_context exclusively, and not rely on the passed-in format.
    This unfortunately means that the cache_key changed, so upgrading will invalidate all prior caches. Take note if you rely
    heavily on caching in production when you push this live.

    *DHH*

*   `number_to_percentage` does not crash with `Float::NAN` or `Float::INFINITY`
    as input.

    Fixes #14405.

    *Yves Senn*

*   Take variants into account when calculating template digests in ActionView::Digestor.

    The arguments to ActionView::Digestor#digest are now being passed as a hash
    to support variants and allow more flexibility in the future. The support for
    regular (required) arguments is deprecated and will be removed in Rails 5.0 or later.

    *Piotr Chmolowski, Łukasz Strzałkowski*

*   Fix a problem where the default options for the `button_tag` helper were not
    being applied correctly.

    Fixes #14255.

    *Sergey Prikhodko*

*   Fix ActionView label translation for more than 10 nested elements.

    *Vladimir Krylov*

*   Added `:plain`, `:html` and `:body` options for `render` method. Please see
    the ActionPack release notes for more detail.

    *Prem Sichanugrist*

*   Date select helpers accept a format string for the months selector via the
    new option `:month_format_string`.

    When rendered, the format string gets passed keys `:number` (integer), and
    `:name` (string), in order to be able to interpolate them as in

        '%{name} (%<number>02d)'

    for example.

    This option is motivated by #13618.

    *Xavier Noria*

*   Added `config.action_view.raise_on_missing_translations` to define whether an
    error should be raised for missing translations.

    Fixes #13196.

    *Kassio Borges*

*   Improved ERB dependency detection. New argument types and formattings for the `render`
    calls can be matched.

    Fixes #13074, #13116.

    *João Britto*

*   Use `display:none` instead of `display:inline` for hidden fields.

    Fixes #6403.

    *Gaelian Ditchburn*

*   The `video_tag` helper now accepts a number for `:size`.

    The `:size` option of the `video_tag` helper now can be specified
    with a stringified number. The `width` and `height` attributes of
    the generated tag will be the same.

    *Kuldeep Aggarwal*

*   Escape format, negative_format and units options of number helpers

    Fixes: CVE-2014-0081

*   A Cycle object should accept an array and cycle through it as it would with a set of
    comma-separated objects.

        arr = [1,2,3]
        cycle(arr) # => '1'
        cycle(arr) # => '2'
        cycle(arr) # => '3'

    Previously, it would return the array as a string, because it took the array as a
    single object:

        arr = [1,2,3]
        cycle(arr) # => '[1,2,3]'
        cycle(arr) # => '[1,2,3]'
        cycle(arr) # => '[1,2,3]'

    *Kristian Freeman*

*   Label tags generated by collection helpers only inherit the `:index` and
    `:namespace` from the input, because only these attributes modify the
    `for` attribute of the label. Also, the input attributes don't have
    precedence over the label attributes anymore.

    Before:

        collection = [[1, true, { class: 'foo' }]]
        f.collection_check_boxes :options, collection, :second, :first do |b|
          b.label(class: 'my_custom_class')
        end

        # => <label class="foo" for="user_active_true">1</label>

    After:

        collection = [[1, true, { class: 'foo' }]]
        f.collection_check_boxes :options, collection, :second, :first do |b|
          b.label(class: 'my_custom_class')
        end

        # => <label class="my_custom_class" for="user_active_true">1</label>

    *Andriel Nuernberg*

*   Fix a long-standing bug in `json_escape` that caused quotation marks to be stripped.
    This method also escapes the \u2028 and \u2029 unicode newline characters which are
    treated as \n in JavaScript. This matches the behaviour of the AS::JSON encoder. (The
    original change in the encoder was introduced in #10534.)

    *Godfrey Chan*

*   `ActionView::MissingTemplate` includes underscore when raised for a partial.

    Fixes #13002.

    *Yves Senn*

*   Use `set_backtrace` instead of instance variable `@backtrace` in ActionView exceptions.

    *Shimpei Makimoto*

*   Fix `simple_format` escapes own output when passing `sanitize: true`.

    *Paul Seidemann*

*   Ensure `ActionView::Digestor.cache` is correctly cleaned up when
    combining recursive templates with `ActionView::Resolver.caching = false`.

    *wyaeld*

*   Fix `collection_check_boxes` so the generated hidden input correctly uses the
    name attribute provided in the options hash.

    *Angel N. Sciortino*

*   Fix some edge cases for the AV `select` helper with the `:selected` option.

    *Bogdan Gusiev*

*   Enable passing a block to the `select` helper.

    Example:

        <%= select(report, "campaign_ids") do %>
          <% available_campaigns.each do |c| -%>
            <%= content_tag(:option, c.name, value: c.id, data: { tags: c.tags.to_json }) %>
          <% end -%>
        <% end -%>

    *Bogdan Gusiev*

*   Handle `:namespace` form option in collection labels.

    *Vasiliy Ermolovich*

*   Fix `form_for` when both `namespace` and `as` options are present.

    The `as` option no longer overwrites the `namespace` option when
    generating an HTML id attribute of the form element.

    *Adam Niedzielski*

*   Fix `excerpt` when `:separator` is `nil`.

    *Paul Nikitochkin*

*   Only cache template digests if `config.cache_template_loading` is true.

    *Josh Lauer*, *Justin Ridgewell*

*   Fix a bug where the lookup details were not being taken into account
    when caching the digest of a template - changes to the details now
    cause a different cache key to be used.

    *Daniel Schierbeck*

*   Added an `extname` hash option to the `javascript_include_tag` method.

    Before:

        javascript_include_tag('templates.jst')
        # => <script src="/javascripts/templates.jst.js"></script>

    After:

        javascript_include_tag('templates.jst', extname: false )
        # => <script src="/javascripts/templates.jst"></script>

    *Nathan Stitt*

*   Fix `current_page?` when the URL contains escaped characters and the
    original URL is using the hexadecimal lowercased.

    *Rafael Mendonça França*

*   Fix `text_area` to behave like `text_field` when `nil` is given as a
    value.

    Before:

        f.text_field :field, value: nil #=> <input value="">
        f.text_area :field, value: nil  #=> <textarea>value of field</textarea>

    After:

        f.text_area :field, value: nil  #=> <textarea></textarea>

    *Joel Cogen*

*   Allow `grouped_options_for_select` to optionally contain html attributes
    as the last element of the array.

        grouped_options_for_select(
          [["North America", [['United States','US'],"Canada"], data: { foo: 'bar' }]]
        )

    *Vasiliy Ermolovich*

*   Fix default rendered format problem when calling `render` without :content_type option.
    It should return :html. Fix #11393.

    *Gleb Mazovetskiy*, *Oleg*, *kennyj*

*   Fix `link_to` with block and url hashes.

    Before:

        link_to(action: 'bar', controller: 'foo') { content_tag(:span, 'Example site') }
        # => "<a action=\"bar\" controller=\"foo\"><span>Example site</span></a>"

    After:

        link_to(action: 'bar', controller: 'foo') { content_tag(:span, 'Example site') }
        # => "<a href=\"/foo/bar\"><span>Example site</span></a>"

    *Murahashi Sanemat Kenichi*

*   Fix "Stack Level Too Deep" error when rendering recursive partials.

    Fixes #11340.

    *Rafael Mendonça França*

*   Added an `enforce_utf8` hash option for `form_tag` method.

    Control to output a hidden input tag with name `utf8` without monkey
    patching.

    Before:

        form_tag
        # => '<form>..<input name="utf8" type="hidden" value="&#x2713;" />..</form>'

    After:

        form_tag
        # => '<form>..<input name="utf8" type="hidden" value="&#x2713;" />..</form>'

        form_tag({}, { :enforce_utf8 => false })
        # => '<form>....</form>'

    *ma2gedev*

*   Remove the deprecated `include_seconds` argument from `distance_of_time_in_words`,
    pass in an `:include_seconds` hash option to use this feature.

    *Carlos Antonio da Silva*

*   Remove deprecated block passing to `FormBuilder#new`.

    *Vipul A M*

*   Pick `DateField` `DateTimeField` and `ColorField` values from stringified options
    allowing use of symbol keys with helpers.

    *Jon Rowe*

*   Remove the deprecated `prompt` argument from `grouped_options_for_select`,
    pass in a `:prompt` hash option to use this feature.

    *kennyj*

*   Always escape the result of `link_to_unless` methods.

    Before:

        link_to_unless(true, '<b>Showing</b>', 'github.com')
        # => "<b>Showing</b>"

    After:

        link_to_unless(true, '<b>Showing</b>', 'github.com')
        # => "&lt;b&gt;Showing&lt;/b&gt;"

    *dtaniwaki*

*   Use a case insensitive URI Regexp for #asset_path.

    This fixes a problem where the same asset path using different cases
    was generating different URIs.

    Before:

        image_tag("HTTP://google.com")
        # => "<img alt=\"Google\" src=\"/assets/HTTP://google.com\" />"
        image_tag("http://google.com")
        # => "<img alt=\"Google\" src=\"http://google.com\" />"

    After:

        image_tag("HTTP://google.com")
        # => "<img alt=\"Google\" src=\"HTTP://google.com\" />"
        image_tag("http://google.com")
        # => "<img alt=\"Google\" src=\"http://google.com\" />"

    *David Celis*

*   Allow `collection_check_boxes` and `collection_radio_buttons` to
    optionally contain html attributes as the last element of the array.

    *Vasiliy Ermolovich*

*   Update the HTML `BOOLEAN_ATTRIBUTES` in `ActionView::Helpers::TagHelper`
    to conform to the latest HTML 5.1 spec. Add attributes `allowfullscreen`,
    `default`, `inert`, `sortable`, `truespeed`, `typemustmatch`. Fix attribute
    `seamless` (previously misspelled `seemless`).

    *Alex Peattie*

*   Fix an issue where partials with a number in the filename were not
    being digested for cache dependencies.

    *Bryan Ricker*

*   First release, ActionView extracted from ActionPack.

    *Piotr Sarnacki*, *Łukasz Strzałkowski*

Please check [4-0-stable (ActionPack's CHANGELOG)](https://github.com/rails/rails/blob/4-0-stable/actionpack/CHANGELOG.md) for previous changes.
